# Requisitos Funcionais (RF) - Washii

| ID | Descrição |
| --- | --- |
| **RF01** | O sistema deve permitir o cadastro de usuários diferenciando dois perfis: Cliente e Lava-jato. |
| **RF02** | Todo usuário deve cadastrar informações básicas como e-mail, senha, foto de perfil, status ativo (true/false), um ou mais telefones e endereço básico (cidade e estado). |
| **RF03** | Os estabelecimentos do tipo Lava-jato devem cadastrar dados específicos: CNPJ, razão social, nome fantasia, capacidade de atendimento simultâneo, endereço completo (logradouro, número, bairro, CEP) e coordenadas geográficas (latitude e longitude). |
| **RF04** | Os Lava-jatos devem cadastrar seus dias de expediente, definindo o dia da semana, horário de início e horário de término do trabalho. |
| **RF05** | Os usuários do tipo Cliente devem cadastrar seu nome completo. |
| **RF06** | O Cliente deve poder cadastrar, atualizar e gerenciar um ou mais veículos associados à sua conta. |
| **RF07** | Cada veículo deve conter placa, marca, modelo, cor, categoria (moto, carro, pickup, caminhão, van, SUV) e status ativo (true/false). |
| **RF08** | O Lava-jato deve poder cadastrar, atualizar e gerenciar serviços (nome, descrição, status ativo e categoria do serviço como lavagem, polimento, higienização, etc.). |
| **RF09** | O preço e a duração de cada serviço devem ser customizados por categoria de veículo. A ausência de customização deve indicar que o lava-jato não atende aquela categoria com aquele serviço. |
| **RF10** | O Cliente deve poder solicitar agendamentos escolhendo um veículo, um ou mais serviços, data e hora. |
| **RF11** | O sistema deve calcular automaticamente o preço total e a duração total do agendamento com base nos serviços e na categoria do veículo. |
| **RF12** | O sistema deve registrar o agendamento com status inicial de "agendado". |
| **RF13** | O sistema deve validar e impedir a realização de um agendamento caso o lava-jato atinja ou ultrapasse sua capacidade de atendimento simultâneo no horário escolhido. |
| **RF14** | O sistema deve permitir que o Cliente cancele um agendamento. |
| **RF15** | O sistema deve permitir que o Lava-jato altere o status de um agendamento (como em andamento, concluído ou cancelado). |
| **RF16** | Os Clientes e Lava-jatos devem poder visualizar o histórico de todos os seus agendamentos e filtrá-los por status. |
| **RF17** | O Lava-jato deve poder visualizar os agendamentos do dia corrente, ordenados cronologicamente por horário. |
| **RF18** | Os Clientes devem poder avaliar os serviços realizados, informando nível de satisfação, data de publicação e, opcionalmente, um comentário. |
| **RF19** | O sistema deve gerar notificações vinculadas a um agendamento específico, contendo título, mensagem, data, hora, status de leitura e o remetente (cliente ou lava-jato). |
| **RF20** | O Cliente deve poder pesquisar estabelecimentos de lava-jato por localidade e proximidade. |
| **RF21** | O Cliente deve poder pesquisar serviços de lava-jatos por categorias (ex.: lavagem, polimento, etc.). |
| **RF22** | O Lava-jato deve poder pesquisar um agendamento específico informando a placa do veículo. |
| **RF23** | O sistema deve integrar-se a um serviço de mapas e geolocalização para permitir que o cliente visualize a localização do lava-jato e trace rotas de navegação até o estabelecimento. |
| **RF24** | O sistema deve disponibilizar um painel gerencial de relatórios operacionais para os estabelecimentos do tipo lava-jato. |
| **RF25** | O sistema deve exibir ao cliente apenas os horários disponíveis de um lava-jato específico. |

# Requisitos Não Funcionais (RNF)

| ID | Descrição |
| --- | --- |
| **RNF01** | O sistema backend deve ser desenvolvido em Spring Boot. |
| **RNF02** | O acesso aos dados no banco de dados deve ser protegido por políticas de segurança a nível de linha (Row Level Security - RLS). |
| **RNF03** | O sistema deve utilizar um banco de dados relacional PostgreSQL. |
| **RNF04** | A aplicação web/API deve operar em ambiente online para permitir a comunicação em tempo real entre clientes e lava-jatos. |
| **RNF05** | O sistema deve implementar soft delete, a fim de presevar os registro originais no banco e seus históricos | 