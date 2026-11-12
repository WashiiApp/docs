# Especificação de Caso de Uso: Consultar Disponibilidade (UC10)

## 1. Resumo

Este caso de uso descreve o processo pelo qual um **Cliente consulta os dias e horários disponíveis para realizar um ou mais serviços em um lava-jato** no sistema Washii.

O sistema deve considerar o **expediente do estabelecimento, a duração dos serviços selecionados, os horários já ocupados e a capacidade de atendimento simultâneo do lava-jato**, apresentando ao cliente somente os horários que atendam às regras de disponibilidade.

## 2. Atores

* **Ator Principal:** Cliente
* **Ator Secundário:** Lava-jato

## 3. Pré-condições

* O cliente deve estar devidamente cadastrado e autenticado no sistema.
* O lava-jato consultado deve possuir cadastro ativo.
* O lava-jato deve possuir dias e horários de expediente cadastrados.
* O lava-jato deve possuir serviços ativos e com duração configurada.
* O cliente deve ter selecionado pelo menos um serviço para realizar a consulta.

## 4. Pós-condições

* O sistema apresenta ao cliente os dias e horários disponíveis para o serviço.
* Os horários apresentados respeitam o expediente cadastrado pelo lava-jato.
* Os horários consideram a duração total dos serviços selecionados.
* Os horários ocupados são desconsiderados.
* A capacidade de atendimento simultâneo do estabelecimento é considerada na disponibilidade apresentada.
* Nenhum agendamento é criado apenas pela consulta de disponibilidade.

## 5. Fluxo Principal (Caminho Feliz)

1. O cliente acessa a opção de agendamento de um lava-jato.
2. O sistema identifica o lava-jato selecionado.
3. O cliente seleciona um ou mais serviços que deseja realizar.
4. O sistema calcula a **duração total estimada** dos serviços selecionados.
5. O cliente informa ou seleciona a data desejada para o atendimento.
6. O sistema consulta o expediente do lava-jato para a data selecionada.
7. O sistema identifica os horários já ocupados por outros agendamentos.
8. O sistema verifica a capacidade de atendimento simultâneo do lava-jato em cada intervalo de horário.
9. O sistema verifica se existe tempo suficiente dentro do expediente para executar os serviços selecionados.
10. O sistema elimina os horários que estejam fora do expediente, ocupados ou que ultrapassem a capacidade de atendimento simultâneo.
11. O sistema apresenta ao cliente os horários disponíveis para a data selecionada.
12. O cliente pode selecionar um dos horários apresentados e prosseguir com o agendamento.

## 6. Fluxos Alternativos

**FA01: Consulta para outra data**

1. O cliente realiza uma consulta de disponibilidade.
2. O sistema apresenta os horários disponíveis para a data selecionada.
3. O cliente escolhe outra data.
4. O sistema realiza uma nova consulta considerando o expediente, os agendamentos existentes e a capacidade do lava-jato para a nova data.
5. O sistema apresenta os horários disponíveis.

**FA02: Alteração dos serviços selecionados**

1. O cliente consulta a disponibilidade após selecionar os serviços.
2. O cliente adiciona ou remove um serviço.
3. O sistema recalcula a duração total dos serviços.
4. O sistema realiza novamente a verificação de disponibilidade.
5. O sistema apresenta os horários compatíveis com a nova duração.

**FA03: Consulta de disponibilidade sem horários para a data selecionada**

1. O cliente seleciona uma data.
2. O sistema verifica o expediente, os agendamentos existentes e a capacidade do lava-jato.
3. O sistema identifica que não existem horários disponíveis.
4. O sistema informa ao cliente que não há horários disponíveis para a data selecionada.
5. O cliente pode selecionar outra data ou alterar os serviços escolhidos.

## 7. Fluxos de Exceção

**EX01: Data fora do expediente**

1. O cliente seleciona uma data em que o lava-jato não possui expediente cadastrado.
2. O sistema consulta o calendário de funcionamento do estabelecimento.
3. O sistema identifica que o estabelecimento não funciona naquela data.
4. O sistema não apresenta horários disponíveis.
5. O sistema informa: **"O estabelecimento não possui expediente nesta data."**

**EX02: Serviço ultrapassa o horário de expediente**

1. O cliente seleciona um ou mais serviços.
2. O sistema calcula a duração total dos serviços.
3. O cliente seleciona uma data próxima ao encerramento do expediente.
4. O sistema verifica se a duração total do serviço cabe no período restante de funcionamento.
5. O sistema identifica que o serviço ultrapassaria o horário de encerramento.
6. O sistema não disponibiliza esse horário para seleção.

**EX03: Capacidade simultânea atingida**

1. O cliente seleciona uma data e horário.
2. O sistema verifica os agendamentos existentes naquele intervalo.
3. O sistema identifica que a quantidade de atendimentos simultâneos atingiu a capacidade máxima configurada para o lava-jato.
4. O sistema considera o horário indisponível.
5. O sistema não apresenta o horário como opção para o cliente.

**EX04: Horário parcialmente ocupado**

1. O cliente seleciona serviços cuja duração exige determinado intervalo de tempo.
2. O sistema verifica os agendamentos existentes durante todo o intervalo necessário.
3. O sistema identifica que a capacidade disponível é insuficiente em algum momento do intervalo.
4. O sistema considera o horário como indisponível.
5. O sistema apresenta somente horários cujo intervalo completo possua capacidade suficiente.

**EX05: Data ou horário retroativo**

1. O cliente tenta consultar disponibilidade para uma data ou horário que já passou.
2. O sistema compara a data e horário solicitados com a data e horário atuais.
3. O sistema identifica que o período solicitado é retroativo.
4. O sistema impede a seleção do período.
5. O sistema informa: **"Não é possível consultar ou agendar horários anteriores ao momento atual."**

**EX06: Alteração da disponibilidade durante a consulta**

1. O cliente consulta os horários disponíveis.
2. Outro cliente realiza um agendamento enquanto a consulta permanece aberta.
3. A capacidade disponível do horário é alterada.
4. O cliente tenta selecionar o horário que havia sido apresentado anteriormente.
5. O sistema realiza uma nova validação da disponibilidade.
6. O sistema identifica que o horário não está mais disponível.
7. O sistema informa: **"Este horário não está mais disponível."**
8. O sistema apresenta novamente os horários atualmente disponíveis.
