# Especificação de Caso de Uso: Avaliar Serviço Realizado (UC17)

## 1. Resumo

Este caso de uso descreve o processo pelo qual um **Cliente avalia um serviço que foi efetivamente realizado por um Lava-jato no sistema Washii**. O cliente poderá atribuir uma nota ao serviço e, opcionalmente, registrar um comentário sobre a experiência.

O sistema deve garantir que **somente agendamentos com serviço efetivamente realizado possam receber uma avaliação**, impedindo avaliações relacionadas a agendamentos cancelados, pendentes ou ainda não concluídos.

## 2. Atores

* **Ator Principal:** Cliente
* **Ator Secundário:** Lava-jato

## 3. Pré-condições

* O cliente deve estar devidamente cadastrado e autenticado no sistema.
* O cliente deve possuir pelo menos um agendamento associado à sua conta.
* O agendamento deve possuir status **"realizado"**.
* O serviço avaliado deve estar vinculado ao cliente que está realizando a avaliação.

## 4. Pós-condições

* Uma nova avaliação é registrada no banco de dados.
* A avaliação fica vinculada ao cliente, ao lava-jato e ao serviço/agendamento realizado.
* A nota atribuída e o comentário, quando informado, são armazenados.
* A avaliação passa a compor a média de avaliações do estabelecimento e/ou serviço, conforme as regras do sistema.
* O lava-jato pode ser notificado sobre a nova avaliação, caso exista mecanismo de notificação configurado.

## 5. Fluxo Principal (Caminho Feliz)

1. O cliente acessa a área de **"Meus Agendamentos"**.
2. O sistema exibe os agendamentos vinculados ao cliente.
3. O cliente seleciona um agendamento com status **"realizado"**.
4. O sistema verifica se o agendamento atende às condições necessárias para receber uma avaliação.
5. O sistema exibe a opção **"Avaliar Serviço"**.
6. O cliente seleciona a nota que deseja atribuir ao serviço, conforme a escala definida pelo sistema.
7. O cliente informa, opcionalmente, um comentário sobre o serviço realizado.
8. O cliente confirma o envio da avaliação.
9. O sistema valida os dados informados na avaliação.
10. O sistema verifica novamente se o serviço possui status **"realizado"** e se está apto a ser avaliado.
11. O sistema registra a avaliação vinculada ao agendamento, cliente, lava-jato e serviço.
12. O sistema atualiza os dados de avaliação do lava-jato e/ou serviço, quando aplicável.
13. O sistema exibe uma mensagem de confirmação informando que a avaliação foi registrada com sucesso.

## 6. Fluxos Alternativos

**FA01: Avaliar serviço sem adicionar comentário**

1. O cliente seleciona um agendamento com status **"realizado"**.
2. O sistema exibe o formulário de avaliação.
3. O cliente informa somente a nota.
4. O cliente confirma o envio.
5. O sistema valida e registra a avaliação sem comentário.
6. O sistema exibe a confirmação de sucesso.

**FA02: Cancelar avaliação antes do envio**

1. O cliente acessa o formulário de avaliação.
2. O cliente seleciona uma nota e/ou informa um comentário.
3. O cliente decide não prosseguir com a avaliação.
4. O cliente cancela a operação.
5. O sistema descarta os dados ainda não enviados.
6. O sistema retorna à tela do agendamento.

## 7. Fluxos de Exceção

**EX01: Serviço ainda não foi realizado**

1. O cliente tenta avaliar um agendamento que ainda está com status diferente de **"realizado"**.
2. O sistema verifica o status atual do agendamento.
3. O sistema impede o registro da avaliação.
4. O sistema exibe uma mensagem informando: **"Este serviço ainda não foi realizado e não pode ser avaliado."**
5. O sistema retorna o cliente para a lista de agendamentos.

**EX02: Agendamento cancelado**

1. O cliente tenta acessar a avaliação de um agendamento cancelado.
2. O sistema identifica que o status do agendamento é **"cancelado"**.
3. O sistema impede a realização da avaliação.
4. O sistema informa que serviços cancelados não podem ser avaliados.

**EX03: Avaliação já realizada**

1. O cliente seleciona um serviço que já possui uma avaliação registrada pelo mesmo cliente.
2. O sistema identifica a existência de uma avaliação vinculada ao agendamento.
3. O sistema impede o cadastro de uma nova avaliação para o mesmo serviço/agendamento.
4. O sistema informa que aquele serviço já foi avaliado.

**EX04: Nota não informada**

1. O cliente acessa o formulário de avaliação.
2. O cliente tenta confirmar a avaliação sem informar uma nota.
3. O sistema identifica que o campo obrigatório não foi preenchido.
4. O sistema impede o envio.
5. O sistema exibe uma mensagem solicitando que o cliente informe uma nota antes de continuar.

**EX05: Dados da avaliação inválidos**

1. O cliente informa dados de avaliação fora dos critérios estabelecidos pelo sistema.
2. O sistema valida os dados enviados.
3. O sistema identifica a inconsistência.
4. O sistema impede o registro da avaliação.
5. O sistema informa ao cliente quais dados precisam ser corrigidos.

**EX06: Serviço deixa de estar elegível durante o envio**

1. O cliente abre o formulário de avaliação quando o serviço está apto a ser avaliado.
2. Antes da confirmação, o estado do agendamento é alterado no sistema.
3. O cliente confirma a avaliação.
4. O sistema realiza uma nova validação do status do agendamento.
5. O sistema identifica que o serviço não está mais com status **"realizado"**.
6. O sistema impede o registro da avaliação.
7. O sistema informa que o serviço não pode mais ser avaliado.
