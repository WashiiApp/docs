# Washii - Gerenciador de Agendamentos para Lava-jatos

Washii é um sistema web integrado e online desenvolvido para otimizar o gerenciamento de agendamentos em lava-jatos. A plataforma conecta clientes e estabelecimentos, automatizando o processo de marcação de horários, controle de veículos, seleção de serviços e o gerenciamento da capacidade de atendimento simultâneo de cada lava-jato.

O sistema baseia-se na criação de contas de usuários, que podem ser classificadas em dois perfis: Cliente ou Lava-jato. Todo usuário do sistema deve possuir as seguintes informações básicas: e-mail, senha, foto de perfil, status ativo (true ou false), um ou mais telefones e endereço básico (cidade e estado).

Os estabelecimentos do tipo Lava-jato contam com dados adicionais: CNPJ, razão social, nome fantasia, capacidade de atendimento simultâneo e endereço completo (composto por logradouro, número, bairro, CEP e coordenadas geográficas de latitude e longitude). Além disso, o sistema registra os dias de expediente do estabelecimento, mapeando o dia da semana (segunda a domingo), o horário de início e o horário de término do trabalho.

Os usuários do tipo Cliente possuem nome completo e podem gerenciar um ou mais veículos. Cada veículo é caracterizado por placa, marca, modelo, cor, categoria (moto, carro, pickup, caminhão, van, SUV) e status ativo (true ou false).

Cada lava-jato pode oferecer um ou mais serviços. Cada serviço possui nome, descrição, status ativo (true ou false) e categoria (lavagem, polimento, higienização, etc.). O preço e a duração do serviço são customizados por uma ou mais categorias de veículo (a ausência de customização indica que o serviço não atende àquela categoria específica). Adicionalmente, os serviços podem ser avaliados por um ou mais clientes, contendo nível de satisfação, data de publicação e, opcionalmente, um comentário.

Os clientes podem solicitar agendamentos. Cada agendamento associa um único veículo, um ou mais serviços, data, hora, preço total, duração total e status (agendado, concluído ou cancelado). O sistema deve garantir que nenhum agendamento seja registrado caso o estabelecimento ultrapasse sua capacidade de atendimento simultâneo no horário escolhido.

Por fim, o sistema gerencia o disparo de notificações. Cada notificação possui título, mensagem, data, hora, status de leitura (true ou false) e remetente (lava-jato ou cliente). As notificações estão estritamente vinculadas a um agendamento específico, sendo direcionadas do cliente para o lava-jato, ou vice-versa.