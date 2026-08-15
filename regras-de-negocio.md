# Regras de Negócio (RN) - Washii

* **RN01 - Validação de Capacidade Simultânea:** 
  O sistema deve impedir a confirmação de um agendamento se a quantidade de atendimentos ativos (com status "agendado" ou "em andamento") no mesmo estabelecimento e horário atingir ou ultrapassar o limite de capacidade simultânea cadastrado pelo lava-jato.

* **RN02 - Customização de Serviços por Veículo:** 
  O preço e a duração de um serviço são estipulados por categoria de veículo. Caso o lava-jato não possua um registro de preço/duração para determinada categoria, o sistema deve bloquear a seleção daquele serviço para o veículo correspondente.

* **RN03 - Restrição de Horário de Expediente:** 
  Agendamentos só podem ser realizados dentro dos dias da semana e horários de expediente (início e término) previamente cadastrados pelo estabelecimento.

* **RN04 - Ciclo de Vida do Agendamento:** 
  Todo agendamento nasce obrigatoriamente com o status inicial `"agendado"`. Ele pode transacionar para `"em andamento"` ou `"concluído"` exclusivamente pelo lava-jato, ou para `"cancelado"` por ambas as partes (cliente ou lava-jato).