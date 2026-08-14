# Washii - Gerenciador de Agendamentos para Lava-jatos

Washii é um sistema informatizado e online para
o gerenciamento de agendamentos de serviços em lava-jatos.

O sistema deve permitir a criação de contas para usuários, que podem ser um dos
tipos: clientes ou lava-jato.Todo usuário deve possuir as seguintes informações: e-mail, senha, foto de perfil, ativo (true ou false), um ou mais telefones e endereço básico (cidade e estado).

Os usuários do tipo lava-jato devem conter CNPJ, razão social, nome fantasia,
capacidade de atendimento simultâneo, endereço completo composto por:
logradouro, número, bairro, CEP e as coordenadas geográficas (latitude e
longitude). Além disso, é necessário registrar os dias de trabalho do
estabelecimento, onde cada registro mapeia o dia da semana (seg, ter, qua, qui,
sex, sab, dom), o horário de início e o horário de término do expediente.

Os usuários do tipo Cliente devem conter nome completo. Cada cliente pode ter um
ou mais veículos. Cada veículo deve conter placa, marca, modelo, cor, categoria
(moto, carro, pickup, caminhão, van, suv) e ativo (true ou false). 

Cada lava-jato pode conter um ou mais serviços. Cada serviço deve conter nome,
descrição, ativo (true ou false) e a categoria (lavagem, polimento, higienização, etc).
O serviço deverá ter seu preço e duração customizados por uma ou muitas
categorias de veículo (a não customização indicará que o serviço não aceita essa
categoria). Além disso, ele pode ser avaliado por um ou mais clientes, cada avaliação deve conter o nível de satisfação, data de publicação e, opcionalmente, um comentário.

Os clientes podem solicitar agendamentos. Cada agendamento associa um único
veículo, um ou mais serviços, data, hora, preço total, duração total e status
(agendado, concluído ou cancelado). O sistema deve garantir que nenhum
agendamento seja registrado caso o lava-jato ultrapasse sua capacidade de
atendimento simultâneo no horário escolhido.

Por fim, o sistema gerencia o disparo de notificações. Cada notificação possui título,
mensagem, data, hora, lida (true ou false) e remetente (lava jato ou cliente). As
notificações devem estar vinculadas a um agendamento específico. Uma notificação
associada a um agendamento é direcionada de um cliente para um lava-jato, ou
vice-versa.
