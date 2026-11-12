# Especificação de Caso de Uso: Realizar Agendamento (UC01)

## 1. Resumo

Este caso de uso descreve o processo pelo qual um Cliente seleciona um estabelecimento, escolhe os serviços desejados para o seu veículo e efetua o agendamento no sistema Washii, respeitando as regras de disponibilidade, horários de expediente e capacidade simultânea do lava-jato.

## 2. Atores

- **Ator Principal:** Cliente
- **Ator Secundário:** Lava-jato

## 3. Pré-condições

- O cliente deve estar devidamente cadastrado e autenticado no sistema.
- O lava-jato escolhido deve possuir cadastro ativo, serviços configurados e dias de expediente cadastrados.

## 4. Pós-condições

- Um novo registro de agendamento é salvo no banco de dados com o status inicial "agendado".
- O preço e a duração total do serviço são calculados e salvos com base na categoria do veículo.
- Uma notificação automática é disparada e enviada para o lava-jato correspondente.
- A capacidade de atendimento simultâneo do lava-jato é alterada naquele horário específico.

## 5. Fluxo Principal (Caminho Feliz)

1. O cliente acessa a interface de pesquisa de estabelecimentos e busca por um lava-jato.
2. O sistema exibe a lista de lava-jatos disponíveis, que podem ser filtrados pelo cliente.
3. O cliente seleciona um lava-jato específico.
4. O sistema exibe os serviços oferecidos pelo lava-jato.
5. O cliente seleciona um ou mais serviços desejados
6. O cliente seleciona o veículo correspondente da sua frota.
7. O sistema valida a compatibilidade do serviço com a categoria do veículo (conforme a [RN02](../regras-de-negocio.md#rn02)).
8. O cliente escolhe uma data e um horário disponíveis no expediente do estabelecimento.
9.  O sistema calcula automaticamente o preço total e a duração total estimada do agendamento.
10. O sistema valida a disponibilidade do horário perante o expediente e verifica a capacidade de atendimento simultâneo do lava-jato (conforme as regras [RN01](../regras-de-negocio.md#rn01) e [RN03](../regras-de-negocio.md#rn03)).
11. O sistema registra o agendamento com o status inicial "agendado", aplicando o ciclo de vida padrão definido na [RN04](../regras-de-negocio.md#rn04).
12. O sistema gera e envia uma notificação automática para o lava-jato vinculada ao agendamento criado.
13. O sistema exibe uma mensagem de confirmação de sucesso para o cliente.

## 6. Fluxos Alternativos
 **FA01: Cadastrar Veículo durante o Agendamento:** 
  1. O cliente nota que o veículo desejado não está na lista.
  2. O cliente clica em "Cadastrar Novo Veículo".
  3. O sistema abre o formulário de cadastro (Placa, Modelo, Cor).
  4. O cliente preenche os dados e confirma.
  5. O sistema salva o veículo e retorna ao Passo 6 do fluxo principal.

## 7. Fluxos de Exceção
 **EX01: Veículo incompatível com o serviço**
 1. No passo 5 e 6, o cliente selecionar um serviço que não possui customização de preço/duração para a categoria do seu veículo.

 2. O sistema impede a seleção.

 3. O sistema exibe uma mensagem de alerta informando que o estabelecimento não executa aquele serviço para aquele tipo de veículo.

 4. O sistema solicita a alteração da escolha do serviço ou do veículo.

 **EX02: Horário Fica Indisponível** 
 1. Outro usuário reserva o mesmo horário segundos antes da confirmação deste cliente.
 2. O sistema valida os dados e identifica que o capacidade máxima do estabelecimento foi atingida.
 3. O sistema exibe um alerta: "Este horário não está mais disponível".
 4. O sistema retorna o cliente para o Passo 8 para a escolha de um novo horário.

**EX03: Horário fora do expediente**

1. No passo 8/10, o cliente tentar agendar em um dia ou horário em desacordo com o expediente cadastrado.
2. O sistema impede a escolha do horário.
3. O sistema exibe um alerta informando que não é possível realizar agendamentos fora do expediente do estabelecimento.

**EX04: Data ou horário Retroativo**
1. Cliente tenta agendar em um dia ou horário que já passou.
2. O sistema impede a seleção do horário.
3. O sistema exibe um alerta informando que não é possível realizar agendamentos retroativos.