# Especificação de Caso de Uso: Gerenciar Serviços (UC06)
 
## 1. Resumo
 
Este caso de uso descreve o processo pelo qual o Lava-jato cadastra, edita, ativa/desativa e configura os serviços oferecidos em seu catálogo, incluindo a definição de preço e duração específicos por categoria de veículo atendida.
 
## 2. Atores
 
- **Ator Principal:** Lava-jato
## 3. Pré-condições
 
- O lava-jato deve estar devidamente cadastrado e autenticado no sistema.
- Deve existir ao menos uma Categoria de Serviço e uma Categoria de Veículo cadastradas no sistema (dados de domínio pré-existentes).
## 4. Pós-condições
 
- O serviço é criado ou atualizado no catálogo do lava-jato.
- Os preços e durações por categoria de veículo ficam associados ao serviço.
- Serviços marcados como inativos deixam de aparecer na pesquisa pública (UC08), mas permanecem visíveis nos agendamentos já vinculados a eles.
## 5. Fluxo Principal (Caminho Feliz) — Cadastrar novo serviço
 
1. O lava-jato acessa a área de gerenciamento de catálogo de serviços.
2. O sistema exibe a lista de serviços já cadastrados  e a opção de "Adicionar Novo Serviço".
3. O lava-jato seleciona "Adicionar Novo Serviço".
4. O sistema exibe o formulário de cadastro, solicitando nome, descrição e a categoria de serviço (dentre as categorias pré-cadastradas).
5. O lava-jato preenche nome, descrição e seleciona a categoria de serviço.
6. O sistema exibe a lista de categorias de veículo disponíveis, permitindo ao lava-jato selecionar quais categorias serão atendidas por aquele serviço.
7. Para cada categoria de veículo selecionada, o lava-jato informa o preço e a duração daquele serviço para aquela categoria (conforme [RN02](../regras-de-negocio.md#rn02)).
8. O lava-jato confirma o cadastro.
9. O sistema valida se todos os campos obrigatórios foram preenchidos e se ao menos uma categoria de veículo foi configurada.
10. O sistema salva o serviço com status ativo por padrão.
11. O sistema exibe uma mensagem de confirmação de sucesso e retorna à lista de serviços, agora atualizada.
## 6. Fluxos Alternativos
 
**FA01: Editar serviço existente**
1. O lava-jato seleciona um serviço já cadastrado na lista.
2. O sistema exibe o formulário preenchido com os dados atuais do serviço, incluindo os preços/durações por categoria de veículo.
3. O lava-jato altera os campos desejados.
4. O lava-jato confirma a alteração.
5. O sistema valida e salva as alterações, retornando ao Passo 11 do fluxo principal.
   > Observação: alterações de preço não afetam agendamentos já realizados, pois estes mantêm o valor congelado no momento da contratação.

**FA02: Adicionar ou remover categoria de veículo atendida**
1. Em um serviço já cadastrado, o lava-jato adiciona uma nova categoria de veículo à lista de categorias atendidas, informando preço e duração.
2. Alternativamente, o lava-jato remove uma categoria de veículo previamente configurada, deixando de atender aquele tipo de veículo para aquele serviço a partir daquele momento (situação de não atendimento).
3. O sistema salva a alteração, retornando ao Passo 11 do fluxo principal.

**FA03: Ativar ou desativar serviço**
1. O lava-jato seleciona a opção de ativar ou desativar um serviço existente.
2. O sistema altera o status do serviço.
3. Serviços desativados deixam de ser exibidos na pesquisa pública (UC08) imediatamente, mas continuam visíveis para o próprio lava-jato na área de gerenciamento e nos agendamentos já vinculados a ele.
## 7. Fluxos de Exceção
 
**EX01: Campo obrigatório não preenchido**
1. No passo 9, o sistema identifica que um campo obrigatório (nome ou categoria de serviço) não foi preenchido.
2. O sistema impede o salvamento.
3. O sistema exibe uma mensagem indicando os campos pendentes.

**EX02: Nenhuma categoria de veículo configurada**
1. No passo 9, o sistema identifica que o lava-jato não configurou preço/duração para nenhuma categoria de veículo.
2. O sistema impede o salvamento do serviço, pois um serviço sem nenhuma categoria de veículo associada não pode ser contratado (conforme [RN02](../regras-de-negocio.md#rn02)).
3. O sistema exibe uma mensagem solicitando ao menos uma categoria configurada.

**EX03: Nome de serviço duplicado no mesmo lava-jato**
1. No passo 9, o sistema identifica que já existe um serviço com o mesmo nome cadastrado por aquele lava-jato.
2. O sistema impede o salvamento.
3. O sistema exibe uma mensagem informando que já existe um serviço com esse nome no catálogo.

**EX04: Tentativa de excluir serviço com agendamentos vinculados**
1. O lava-jato tenta excluir permanentemente um serviço que já possui agendamentos  associados ainda não concluidos ou não cancelados.
2. O sistema impede a exclusão física do registro.
3. O sistema orienta o lava-jato a desativar o serviço (FA03) em vez de excluí-lo, preservando o histórico de agendamentos.