# Modelagem de Dados Orientada a Documentos (Washii)

Este documento detalha a arquitetura de modelagem de dados implementada para o projeto Washii no MongoDB, explicando o raciocínio por trás de cada decisão, por que uma relação foi embutida ou referenciada, quais padrões de design foram aplicados, e como a estratégia de acesso a dados se compara à modelagem relacional que o projeto tinha originalmente no Supabase/PostgreSQL.

## 1. Visão Geral da Estratégia

A modelagem do Washii para o MongoDB seguiu as três camadas clássicas de modelagem de dados — Conceitual, Lógico e Físico — com uma diferença fundamental em relação ao trabalho já feito para o PostgreSQL: **a camada conceitual é a mesma**, mas a camada lógica parte de uma pergunta que não existe no mundo relacional.

**Camada Conceitual (reaproveitada)**: entidades, atributos e relacionamentos do domínio do negócio (Cliente, LavaJato, Servico, Agendamento etc.) e suas cardinalidades. Esses fatos não mudam entre bancos — o cliente já tinha N veículos e um agendamento já tinha N serviços no modelo relacional, e continuam tendo no MongoDB.

**Camada Lógica (reconstruída)**: no relacional, essa camada normaliza entidades em tabelas e conecta via chave estrangeira. No MongoDB, essa camada decide, relacionamento por relacionamento, se os dados devem ser **embutidos** (juntos no mesmo documento) ou **referenciados** (em coleções separadas, ligados por id) — decisão guiada não pela forma normal, mas pelos padrões de acesso da aplicação (seção 2).

**Camada Física**: os documentos JSON reais, já usando tipos nativos do MongoDB (`ObjectId`, `Date`, GeoJSON) e os índices que sustentam as consultas mapeadas na camada lógica.

> **Princípio norteador:** no modelo relacional, modela-se para normalizar e evitar redundância. No MongoDB, modela-se para atender às consultas da aplicação, aceitando duplicação controlada de dado quando isso evita `joins` custosos ou múltiplas idas ao banco.

## 2. Padrões de Acesso Considerados

- Buscar lava-jatos próximos, já com catálogo e preços.
- Ver perfil do cliente com seus veículos.
- Ver agenda (cliente ou lava-jato) ordenada por data.
- Ver histórico de um agendamento com veículo/serviços congelados no momento da contratação.
- Listar notificações não lidas.
- Listar avaliações e nota média sem agregação em tempo real.
## 3. Decisões de Modelagem por Coleção

### A. Módulo de Perfis (`cliente`, `lavajato`)

- **Duas coleções separadas**, em vez de uma coleção polimórfica única `usuario`. Diferente do modelo relacional — onde `usuario` era uma tabela-mãe compartilhada por herança —, no MongoDB os dois perfis têm listas embutidas completamente diferentes (`veiculos`/`telefones` de um lado, `servicos`/`disponibilidade`/`endereco` de outro). Uma coleção única deixaria metade dos campos sempre vazios em cada documento, dependendo do tipo de usuário.
- **`veiculos[]` embutido em `cliente`**: lista pequena (poucas unidades por cliente) e sempre lida junto com o perfil ao abrir "meus veículos" — não há cenário de acesso que precise de um veículo isolado do seu dono. Cada item carrega `_id` próprio, necessário para ser referenciado depois em `agendamento` (módulo C).
- **`servicos[]`, `disponibilidade[]` embutidos em `lavajato`**: catálogo e agenda semanal de um estabelecimento são pequenos (dezenas de itens no máximo) e sempre exibidos junto com o perfil do lava-jato na tela de busca — embutir evita uma consulta extra por estabelecimento listado.
- **`endereco.localizacao` em formato GeoJSON** (`{ type: "Point", coordinates: [longitude, latitude] }`), no lugar de campos soltos de latitude/longitude — pré-requisito para o índice `2dsphere` e os operadores `$near`/`$geoWithin` usados na busca por proximidade.

### B. Módulo de Catálogo (`servico`, `categoria_servico`, `categoria_veiculo`, `dias_semana`)

- **Tabelas de referência (`categoria_servico`, `categoria_veiculo`, `dias_semana`) viraram valores de texto simples**, embutidos diretamente onde são usados (ex: `categoria_servico: "Lavagem Completa"` dentro de `servico`), em vez de coleções próprias ligadas por id. São listas pequenas e praticamente estáticas — o custo de manter uma coleção e fazer `lookup` a cada leitura não se paga.
- **`precos_por_categoria[]` dentro de cada serviço**: no modelo relacional, `categoria_veiculo_servico` guardava preço e duração por combinação de categoria de veículo e serviço. Essa granularidade foi preservada como uma lista embutida (poucos itens, sempre lidos junto com o serviço).
- **`avaliacao_resumo` embutido em cada serviço**: em vez de embutir a lista completa de avaliações (que cresce sem limite — ver módulo E), mantém-se apenas um resumo (`media`, `total`, `recentes`) para a tela de busca não precisar agregar a coleção `avaliacao` toda vez que lista estabelecimentos.

### C. Módulo Transacional (`agendamento`)

- **Snapshot de `veiculo` e `servicos[]` embutido no próprio agendamento**, com o id de referência original preservado (`id_veiculo`, `id_servico`). A cópia dos dados (nome, preço, duração) no momento da contratação captura uma regra de negócio real: se o lava-jato alterar o preço de um serviço amanhã, o agendamento de ontem não deve mudar de valor retroativamente. Manter o id junto permite rastrear a origem do snapshot para relatórios, sem depender só do texto copiado.
- **`data` e `hora` unificados em `data_hora`** (tipo `Date` nativo), permitindo ordenação e consultas por intervalo sem parsing manual de string.
- Campos que só fazem sentido no cadastro "vivo" (`ativo`, `descricao`) foram deixados de fora do snapshot — um registro histórico não deve mudar de estado porque o cadastro original mudou.

### D. Módulo de Notificações (`notificacao`)

- **Extraída para uma coleção própria**, referenciando o destinatário (`id_destinatario` + `tipo_destinatario`) em vez de ficar embutida dentro de `cliente`/`lavajato`. Notificação é criada a cada evento e nunca é removida organicamente — embutida, faria o documento do usuário crescer indefinidamente, contrariando o limite prático de tamanho de documento do MongoDB e degradando a performance de escrita a cada novo evento.
- **Coleção única para os dois tipos de destinatário**, com discriminador `tipo_destinatario`, em vez de duas coleções separadas — a tela de "caixa de entrada" busca notificações independente de quem é o usuário logado.
- **Índice TTL** configurado para expirar notificações automaticamente após um período, dispensando rotina de limpeza manual.

### E. Módulo de Avaliações (`avaliacao`)

- **Mesmo raciocínio de `notificacao`**: uma avaliação por par cliente-serviço, criada e nunca removida — coleção própria em vez de lista embutida no serviço.
- **`id_agendamento` como trava de regra de negócio**: garante, na camada de aplicação, que só quem teve um agendamento concluído pode avaliar aquele serviço.
- Índice único em `(id_cliente, id_servico)` reproduz a mesma restrição que existia no modelo relacional.

## 4. Padrões de Design Aplicados

| Padrão | Onde foi usado | Motivo |
|---|---|---|
| **Embed (Embutir)** | `veiculos` em `cliente`; `servicos`/`disponibilidade` em `lavajato`; snapshot em `agendamento` | Dados pequenos, lidos sempre junto com o documento pai |
| **Reference (Referenciar)** | `notificacao`, `avaliacao` como coleções próprias | Listas de tamanho ilimitado, que cresceriam para sempre se embutidas |
| **Subset Pattern** | `avaliacao_resumo.recentes` (só as últimas avaliações, não todas) | Evita embutir uma lista grande, mantendo só uma amostra útil pra leitura rápida |
| **Computed Pattern** | `avaliacao_resumo.media`/`total` | Valor pré-calculado pela aplicação a cada escrita em `avaliacao`, evitando agregação em tempo real na leitura |


## 5. Estrutura Final das Coleções

| Coleção | Descrição | Embutido | Referenciado |
|---|---|---|---|
| `cliente` | Perfil do cliente | `veiculos[]`, `telefones[]` | — |
| `lavajato` | Perfil do estabelecimento | `servicos[]`, `disponibilidade[]`, `endereco` | — |
| `agendamento` | Registro histórico de uma contratação | snapshot de `veiculo`, `servicos[]` | `id_cliente`, `id_lavajato` |
| `notificacao` | Evento de comunicação com um usuário | — | `id_destinatario`, `id_agendamento` |
| `avaliacao` | Feedback de um cliente sobre um serviço | — | `id_cliente`, `id_servico`, `id_agendamento` |