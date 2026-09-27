# Especificação de Caso de Uso: Gerenciar Veículos (UC05)

## 1. Resumo

Este caso de uso descreve o processo pelo qual um **Cliente gerencia os veículos associados à sua conta** no sistema Washii.

A funcionalidade permite ao cliente **cadastrar, consultar, atualizar, ativar e desativar veículos**, mantendo sua frota organizada e disponível para utilização durante os agendamentos de serviços.

O sistema deve garantir que cada veículo esteja corretamente associado à conta do cliente e que os dados obrigatórios sejam validados antes de qualquer operação de cadastro ou alteração.

## 2. Atores

* **Ator Principal:** Cliente
* **Ator Secundário:** Sistema Washii

## 3. Pré-condições

* O cliente deve estar devidamente cadastrado e autenticado no sistema.
* Para realizar alterações, o veículo deve estar associado à conta do cliente.
* Para cadastrar um novo veículo, os dados obrigatórios devem ser informados pelo cliente.

## 4. Pós-condições

* Um novo veículo pode ser cadastrado e associado à conta do cliente.
* Os dados de um veículo existente podem ser atualizados.
* Um veículo pode ser ativado ou desativado.
* A associação entre o veículo e a conta do cliente é mantida corretamente.
* Veículos desativados deixam de ser disponibilizados para novos agendamentos.
* Os registros de veículos permanecem armazenados no sistema, conforme as regras de negócio aplicáveis.

## 5. Fluxo Principal (Caminho Feliz)

1. O cliente acessa a opção **"Meus Veículos"**.
2. O sistema identifica a conta do cliente autenticado.
3. O sistema consulta os veículos associados à conta.
4. O sistema exibe a lista de veículos cadastrados, indicando seu status atual.
5. O cliente seleciona uma operação: **cadastrar, visualizar, atualizar, ativar ou desativar veículo**.
6. Para cadastrar um novo veículo, o cliente seleciona a opção **"Adicionar Veículo"**.
7. O sistema apresenta o formulário de cadastro.
8. O cliente informa os dados solicitados, como **placa, modelo, cor e categoria**, conforme os campos definidos pelo sistema.
9. O sistema valida os dados informados.
10. O sistema verifica se o veículo já está cadastrado ou associado de forma incompatível com a conta.
11. O sistema registra o novo veículo.
12. O sistema associa o veículo à conta do cliente.
13. O sistema define o veículo como **ativo**.
14. O sistema exibe uma mensagem informando que o veículo foi cadastrado com sucesso.

## 6. Fluxos Alternativos

**FA01: Atualizar dados do veículo**

1. O cliente acessa a lista de veículos.
2. O cliente seleciona um veículo ativo ou inativo associado à sua conta.
3. O cliente seleciona a opção **"Editar"**.
4. O sistema apresenta os dados atuais do veículo.
5. O cliente altera os dados permitidos.
6. O cliente confirma a alteração.
7. O sistema valida os novos dados.
8. O sistema atualiza o cadastro do veículo.
9. O sistema exibe uma mensagem de confirmação.

**FA02: Desativar veículo**

1. O cliente acessa a lista de veículos.
2. O cliente seleciona um veículo ativo.
3. O cliente seleciona a opção **"Desativar"**.
4. O sistema solicita a confirmação da operação.
5. O cliente confirma a desativação.
6. O sistema altera o status do veículo para **"inativo"**.
7. O sistema impede que o veículo seja selecionado para novos agendamentos.
8. O sistema informa que o veículo foi desativado com sucesso.

**FA03: Ativar veículo**

1. O cliente acessa a lista de veículos.
2. O cliente seleciona um veículo inativo.
3. O cliente seleciona a opção **"Ativar"**.
4. O sistema verifica se os dados obrigatórios do veículo permanecem válidos.
5. O sistema altera o status do veículo para **"ativo"**.
6. O veículo volta a ficar disponível para utilização em novos agendamentos.
7. O sistema exibe uma mensagem de confirmação.

**FA04: Visualizar detalhes do veículo**

1. O cliente acessa a lista de veículos.
2. O cliente seleciona um veículo.
3. O sistema apresenta os dados cadastrados, incluindo as informações de identificação e o status do veículo.
4. O cliente pode retornar à lista ou selecionar uma operação disponível.

## 7. Fluxos de Exceção

**EX01: Dados obrigatórios não informados**

1. O cliente acessa o formulário de cadastro ou edição.
2. O cliente deixa um ou mais campos obrigatórios sem preenchimento.
3. O cliente tenta confirmar a operação.
4. O sistema identifica a ausência dos dados obrigatórios.
5. O sistema impede o cadastro ou atualização.
6. O sistema destaca os campos que precisam ser preenchidos.

**EX02: Placa já cadastrada**

1. O cliente informa a placa de um veículo durante o cadastro.
2. O sistema verifica os veículos existentes.
3. O sistema identifica que já existe um cadastro com a mesma placa.
4. O sistema impede o cadastro de um novo veículo com a mesma identificação.
5. O sistema informa: **"Este veículo já está cadastrado no sistema."**

**EX03: Dados do veículo inválidos**

1. O cliente informa os dados do veículo.
2. O sistema realiza a validação dos campos.
3. O sistema identifica que um ou mais dados estão em formato inválido.
4. O sistema impede a conclusão da operação.
5. O sistema informa quais dados precisam ser corrigidos.

**EX04: Veículo não pertence à conta do cliente**

1. O cliente tenta acessar ou alterar um veículo por meio de uma referência que não está associada à sua conta.
2. O sistema verifica a associação entre o veículo e o cliente autenticado.
3. O sistema identifica que o veículo não pertence à conta.
4. O sistema impede a operação.
5. O sistema informa que o veículo não está associado à conta do cliente.

**EX05: Tentativa de desativar veículo utilizado em agendamento futuro**

1. O cliente seleciona um veículo ativo para desativação.
2. O sistema verifica se existem agendamentos futuros associados ao veículo.
3. O sistema identifica a existência de um ou mais agendamentos futuros.
4. O sistema impede a desativação ou solicita ao cliente que trate os agendamentos vinculados, conforme a regra de negócio definida.
5. O sistema informa que o veículo possui agendamentos futuros associados.

**EX06: Veículo inativo selecionado para novo agendamento**

1. O cliente inicia um novo agendamento.
2. O sistema consulta os veículos associados à conta.
3. O sistema identifica que o veículo selecionado está com status **"inativo"**.
4. O sistema impede a utilização do veículo no novo agendamento.
5. O sistema informa que o veículo está inativo e não pode ser utilizado.

**EX07: Falha ao salvar os dados**

1. O cliente confirma o cadastro ou atualização do veículo.
2. O sistema realiza as validações necessárias.
3. Ocorre uma falha durante a gravação dos dados.
4. O sistema não confirma a operação como concluída.
5. O sistema informa ao cliente que não foi possível concluir a operação.
6. O sistema mantém os dados anteriores sem alteração, quando aplicável.
