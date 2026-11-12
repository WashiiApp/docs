# Especificação de Caso de Uso: Gerenciar Status do Agendamento (UC13)

## 1. Resumo

Este caso de uso descreve como o Lava-jato transita um agendamento entre os estados do seu ciclo de vida, confirmando o atendimento como concluído ou registrando um cancelamento por parte do estabelecimento.

## 2. Atores

- **Ator Principal:** Lava-jato
- **Ator Secundário:** Cliente (recebe a notificação da mudança de status)

## 3. Pré-condições

- O lava-jato deve estar devidamente cadastrado e autenticado no sistema.
- Deve existir ao menos um agendamento com status "agendado" vinculado ao lava-jato.

## 4. Pós-condições

- O status do agendamento é atualizado no banco de dados.
- Uma notificação automática é enviada ao cliente informando a mudança de status.
- Caso o novo status seja "concluído", o agendamento passa a estar disponível para avaliação pelo cliente (UC17).
- Caso o novo status seja "cancelado", a capacidade de atendimento simultâneo do horário correspondente é liberada (conforme [RN03](../regras-de-negocio.md#rn03)).

## 5. Fluxo Principal (Caminho Feliz) — Concluir agendamento

1. O lava-jato acessa a lista de agendamentos do dia (UC15) ou o histórico de agendamentos (UC14).
2. O sistema exibe os agendamentos com status "agendado" com a opção de alterar o status.
3. O lava-jato seleciona um agendamento e escolhe a opção "Marcar como Concluído".
4. O sistema solicita a confirmação da ação.
5. O lava-jato confirma.
6. O sistema atualiza o status do agendamento para "concluído" (conforme [RN04](../regras-de-negocio.md#rn04)).
7. O sistema gera e envia uma notificação automática ao cliente informando a conclusão do serviço.
8. O sistema exibe uma mensagem de confirmação de sucesso ao lava-jato.

## 6. Fluxos Alternativos

**FA01: Cancelar agendamento pelo lava-jato**
1. O lava-jato seleciona um agendamento com status "agendado" e escolhe a opção "Cancelar Agendamento".
2. O sistema solicita a confirmação e, opcionalmente, um motivo do cancelamento.
3. O lava-jato confirma.
4. O sistema atualiza o status do agendamento para "cancelado" (conforme [RN04](../regras-de-negocio.md#rn04)).
5. O sistema libera a capacidade de atendimento simultâneo referente àquele horário (conforme [RN03](../regras-de-negocio.md#rn03)).
6. O sistema gera e envia uma notificação automática ao cliente informando o cancelamento, retornando ao Passo 8 do fluxo principal.

## 7. Fluxos de Exceção

**EX01: Tentativa de alterar status de agendamento já finalizado**
1. O lava-jato tenta alterar o status de um agendamento que já está "concluído" ou "cancelado".
2. O sistema impede a alteração, pois esses são estados finais do ciclo de vida (conforme [RN04](../regras-de-negocio.md#rn04)).
3. O sistema exibe uma mensagem informando que o agendamento já foi finalizado e não pode ser alterado.

**EX02: Tentativa de conclusão antes do horário agendado**
1. O lava-jato tenta marcar como "concluído" um agendamento cuja data/hora ainda não chegou.
2. O sistema exibe um alerta de confirmação, perguntando se o lava-jato realmente deseja concluir um atendimento antecipadamente.
3. Caso confirmado, o sistema prossegue normalmente a partir do Passo 6 do fluxo principal.

