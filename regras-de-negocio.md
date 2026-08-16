# Regras de Negócio (RN) - Washii

* **RN01** 
  **Validação de Capacidade Simultânea:** O sistema deve impedir a confirmação de um agendamento se a quantidade de atendimentos ativos (com status "agendado" ou "em andamento") no mesmo estabelecimento e horário atingir ou ultrapassar o limite de capacidade simultânea cadastrado pelo lava-jato.

* **RN02** 
  **Customização de Serviços por Veículo:** O preço e a duração de um serviço são estipulados por categoria de veículo. Caso o lava-jato não possua um registro de preço/duração para determinada categoria, o sistema deve bloquear a seleção daquele serviço para o veículo correspondente.

* **RN03** 
  **Restrição de Horário de Expediente:** Agendamentos só podem ser realizados dentro dos dias da semana e horários de expediente (início e término) previamente cadastrados pelo estabelecimento.

* **RN04** 
  **Ciclo de Vida do Agendamento:** Todo agendamento nasce obrigatoriamente com o status inicial `"agendado"`. Ele pode transacionar para `"em andamento"` ou `"concluído"` exclusivamente pelo lava-jato, ou para `"cancelado"` por ambas as partes (cliente ou lava-jato).

* **RN05**
  **Validação de Veículo Ativo**: O sistema deve impedir que o Cliente selecione para agendamento um veículo cujo status esteja inativo (ativo = false). Veículos que possuem agendamentos com status `"agendado"` ou `"em andamento"` não podem ter seu status alterado para inativo ou serem excluídos da conta até que o serviço seja concluído ou cancelado.

* **RN06**
**Unicidade e Validação de Estabelecimento:** O sistema deve validar matematicamente o formato do CNPJ no momento do cadastro do Lava-jato e impedir o registro de duas contas diferentes utilizando o mesmo número de CNPJ. Além disso, o e-mail cadastrado por qualquer tipo de usuário deve ser único em todo o sistema.

* **RN07**
**Elegibilidade para avaliacão:** Apenas clientes autenticados que possuam um agendamento com o status `"concluído"` contendo um determinado serviço estão autorizados a enviar uma avaliação para aquele serviço, sendo vedada a duplicidade de feedback para o mesmo atendimento.