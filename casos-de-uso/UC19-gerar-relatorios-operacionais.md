# Especificação de Caso de Uso: Gerar Relatórios Operacionais (UC19)

## 1. Resumo

Este caso de uso descreve como o Lava-jato configura filtros e gera relatórios operacionais e financeiros consolidados sobre seus agendamentos, serviços e avaliações, para apoiar decisões de gestão do negócio.

## 2. Atores

- **Ator Principal:** Lava-jato

## 3. Pré-condições

- O lava-jato deve estar devidamente cadastrado e autenticado no sistema.
- Deve existir ao menos um agendamento registrado no período consultado para que o relatório apresente dados (caso contrário, é exibido um relatório vazio).

## 4. Pós-condições

- O relatório é gerado e exibido na tela, com opção de exportação.
- Nenhuma alteração é feita nos dados do sistema, este é um caso de uso de consulta/leitura.

## 5. Fluxo Principal (Caminho Feliz)

1. O lava-jato acessa a área de relatórios.
2. O sistema exibe as opções de filtro: período (data inicial e final), status do agendamento, serviço específico e categoria de veículo.
3. O lava-jato define o período desejado e, opcionalmente, os demais filtros.
4. O lava-jato confirma a geração do relatório.
5. O sistema valida os filtros informados (conforme [RN05](../regras-de-negocio.md#rn05)).
6. O sistema consulta os agendamentos, serviços associados e avaliações correspondentes aos filtros aplicados.
7. O sistema calcula os indicadores agregados: faturamento total, quantidade de agendamentos por status, serviços mais solicitados, taxa de cancelamento e nota média de avaliação no período.
8. O sistema exibe o relatório consolidado, com os indicadores e um detalhamento tabular dos agendamentos considerados.
9. O sistema oferece a opção de exportar o relatório gerado.

## 6. Fluxos Alternativos

**FA01: Exportar relatório**
1. Após a exibição do relatório (Passo 8 do fluxo principal), o lava-jato seleciona a opção "Exportar".
2. O sistema solicita o formato desejado (PDF ou CSV).
3. O lava-jato escolhe o formato.
4. O sistema gera o arquivo correspondente e disponibiliza para download.

**FA02: Salvar configuração de filtro como favorita**
1. Após definir os filtros (Passo 3), o lava-jato opta por salvar aquela combinação de filtros como um modelo de relatório favorito, atribuindo um nome.
2. O sistema armazena a configuração para reutilização futura, exibindo-a como atalho na área de relatórios.

## 7. Fluxos de Exceção

**EX01: Período inválido**
1. No passo 5, o sistema identifica que a data inicial informada é posterior à data final (conforme [RN05](../regras-de-negocio.md#rn05)).
2. O sistema impede a geração do relatório.
3. O sistema exibe uma mensagem de erro solicitando a correção do período.

**EX02: Nenhum dado encontrado no período**
1. No passo 6, o sistema não encontra nenhum agendamento correspondente aos filtros aplicados.
2. O sistema exibe o relatório com os indicadores zerados e uma mensagem informando que não há dados para o período/filtros selecionados.

**EX03: Período excede o limite máximo permitido**
1. No passo 5, o sistema identifica que o intervalo entre a data inicial e final excede o limite máximo definido para geração de relatório, o que poderia sobrecarregar o processamento (conforme [RN05](../regras-de-negocio.md#rn05)).
2. O sistema impede a geração do relatório.
3. O sistema exibe uma mensagem sugerindo reduzir o período consultado.