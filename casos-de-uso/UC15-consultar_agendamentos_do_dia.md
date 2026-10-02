Especificação de Caso de Uso: Consultar Agendamentos do Dia (UC15)
1. Resumo

Este caso de uso descreve como o lava-jato consulta os agendamentos previstos para o dia, permitindo visualizar os atendimentos organizados cronologicamente e filtrar os registros conforme o período desejado.

A funcionalidade tem como objetivo auxiliar o lava-jato no controle operacional dos atendimentos, permitindo identificar os próximos serviços, horários ocupados, clientes, veículos, serviços contratados e respectivos status.

2. Atores
Ator Principal: Lava-jato
Ator Secundário: Nenhum
3. Pré-condições
O lava-jato deve estar devidamente cadastrado e autenticado no sistema.
O lava-jato deve possuir acesso à área de gerenciamento de agendamentos.
Devem existir ou poder ser consultados agendamentos vinculados ao lava-jato.
4. Pós-condições
O sistema exibe os agendamentos correspondentes ao período consultado.
Os agendamentos são apresentados em ordem cronológica, considerando o horário de início do atendimento.
O lava-jato consegue visualizar as principais informações de cada agendamento.
Caso existam filtros aplicados, somente os agendamentos compatíveis com os critérios selecionados são apresentados.
Nenhum dado do agendamento é alterado durante a consulta.
5. Fluxo Principal (Caminho Feliz) — Consultar agendamentos do dia
O lava-jato acessa a área de Agendamentos do sistema.
O sistema identifica a data atual como período padrão da consulta.
O sistema busca no banco de dados os agendamentos vinculados ao lava-jato cuja data corresponda ao dia consultado.
O sistema organiza os agendamentos em ordem cronológica crescente, considerando o horário de início do atendimento.
O sistema exibe a lista de agendamentos do dia.
Para cada agendamento, o sistema apresenta, no mínimo:
horário de início;
horário previsto para término;
nome do cliente;
veículo;
placa do veículo;
serviços contratados;
valor total;
status do agendamento.
O lava-jato pode selecionar um agendamento para visualizar seus detalhes.
O sistema apresenta os detalhes do agendamento selecionado.
O lava-jato encerra a consulta ou retorna à lista de agendamentos.
6. Fluxos Alternativos
FA01: Consultar agendamentos de outra data
O lava-jato acessa a área de Agendamentos.
O lava-jato seleciona uma data diferente da data atual.
O sistema recebe a data informada.
O sistema busca os agendamentos vinculados ao lava-jato para a data selecionada.
O sistema organiza os resultados em ordem cronológica crescente.
O sistema apresenta os agendamentos correspondentes à data selecionada.
O lava-jato pode selecionar um agendamento para consultar seus detalhes.
FA02: Filtrar agendamentos por status
O lava-jato acessa a lista de agendamentos.
O lava-jato seleciona um filtro de status.
O sistema disponibiliza os status existentes, como:
Agendado;
Concluído;
Cancelado.
O lava-jato seleciona o status desejado.
O sistema filtra os agendamentos de acordo com o status selecionado.
O sistema apresenta os resultados mantendo a ordenação cronológica.
FA03: Consultar todos os agendamentos
O lava-jato acessa a lista de agendamentos.
O lava-jato remove os filtros aplicados.
O sistema realiza novamente a consulta considerando todos os agendamentos correspondentes ao período selecionado.
O sistema apresenta os resultados em ordem cronológica.
FA04: Selecionar um agendamento
O lava-jato seleciona um agendamento apresentado na lista.
O sistema recupera os dados completos do agendamento.
O sistema apresenta os detalhes do cliente, veículo, serviços, horário, valor e status.
O lava-jato pode retornar para a lista de agendamentos.
7. Fluxos de Exceção
EX01: Nenhum agendamento encontrado
O lava-jato realiza uma consulta para determinada data ou período.
O sistema não encontra nenhum agendamento correspondente aos critérios informados.
O sistema exibe uma mensagem informando que não existem agendamentos para o período selecionado.
O sistema mantém a opção de alterar a data ou remover os filtros.

Exemplo de mensagem:

"Nenhum agendamento encontrado para a data selecionada."

EX02: Falha na consulta dos agendamentos
O lava-jato solicita a consulta dos agendamentos.
O sistema tenta buscar os dados no banco de dados.
Ocorre uma falha de comunicação ou indisponibilidade do serviço.
O sistema não apresenta dados incompletos ou incorretos.
O sistema informa ao lava-jato que não foi possível carregar os agendamentos.
O sistema disponibiliza a opção de tentar novamente.

Exemplo de mensagem:

"Não foi possível carregar os agendamentos. Tente novamente."

EX03: Agendamento não pertence ao lava-jato
O lava-jato tenta acessar diretamente os detalhes de um agendamento.
O sistema verifica a identificação do lava-jato associado ao agendamento.
O sistema identifica que o agendamento não está vinculado ao lava-jato autenticado.
O sistema impede o acesso às informações.
O sistema exibe uma mensagem informando que o agendamento não está disponível para aquele estabelecimento.