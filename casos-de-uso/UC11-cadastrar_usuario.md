Especificação de Caso de Uso: Cadastrar Usuário (UC01)
1. Resumo

Este caso de uso descreve o processo de cadastro de novos usuários no sistema, permitindo que uma pessoa se registre como Cliente ou Lava-jato.

Durante o cadastro, o sistema coleta os dados comuns necessários à identificação e autenticação do usuário e, de acordo com o tipo escolhido, solicita os dados específicos correspondentes.

Ao final do processo, o sistema valida as informações fornecidas, cria o cadastro e disponibiliza o acesso às funcionalidades correspondentes ao perfil selecionado.

2. Atores
Ator Principal: Usuário
Atores Secundários: Nenhum

O ator principal pode realizar o cadastro assumindo um dos seguintes perfis:

Cliente: usuário que utiliza o sistema para consultar lava-jatos, cadastrar veículos e realizar agendamentos.
Lava-jato: estabelecimento que utiliza o sistema para gerenciar seus serviços, horários e agendamentos.
3. Pré-condições
O usuário deve possuir acesso à tela de cadastro.
O usuário não deve possuir um cadastro ativo com o mesmo e-mail informado.
O sistema deve estar disponível para realizar o cadastro.
Os dados obrigatórios devem ser fornecidos pelo usuário.
4. Pós-condições
Um novo usuário é criado no banco de dados.
O usuário é associado ao tipo de perfil selecionado: Cliente ou Lava-jato.
Os dados fornecidos são armazenados de acordo com as regras de segurança e integridade do sistema.
O usuário pode utilizar as funcionalidades correspondentes ao seu perfil após a autenticação.
Caso o cadastro seja realizado com sucesso, o sistema informa o usuário sobre a conclusão do cadastro.
5. Fluxo Principal (Caminho Feliz) — Cadastro de usuário
O usuário acessa a tela de Cadastro.
O sistema apresenta o formulário de cadastro.
O sistema solicita os dados comuns do usuário, como:
primeiro nome;
sobrenome;
e-mail;
senha;
confirmação de senha;
cidade;
estado;
tipo de usuário.
O usuário seleciona o tipo de usuário:
Cliente; ou
Lava-jato.
O sistema identifica o tipo de usuário selecionado.
O sistema apresenta os campos específicos correspondentes ao perfil escolhido.
O usuário preenche os dados solicitados.
O usuário confirma o cadastro.
O sistema valida os dados informados.
O sistema verifica se o e-mail informado já está cadastrado.
O sistema verifica se os dados obrigatórios foram preenchidos corretamente.
O sistema cria o novo usuário.
O sistema registra o usuário com o perfil selecionado.
O sistema apresenta uma mensagem informando que o cadastro foi realizado com sucesso.
O usuário pode prosseguir para a tela de autenticação.
6. Fluxos Alternativos
FA01: Cadastro como Cliente
O usuário acessa a tela de cadastro.
O usuário seleciona a opção Cliente.
O sistema apresenta os campos comuns e os campos específicos necessários ao cliente.
O usuário preenche as informações solicitadas.
O usuário confirma o cadastro.
O sistema valida as informações.
O sistema cria o cadastro do usuário com o tipo CLIENTE.
O sistema informa que o cadastro foi realizado com sucesso.
FA02: Cadastro como Lava-jato
O usuário acessa a tela de cadastro.
O usuário seleciona a opção Lava-jato.
O sistema apresenta os campos comuns e os campos específicos necessários ao estabelecimento.
O usuário preenche as informações solicitadas.
O usuário confirma o cadastro.
O sistema valida as informações.
O sistema cria o cadastro do usuário com o tipo LAVA_JATO.
O sistema informa que o cadastro foi realizado com sucesso.
FA03: Alteração do tipo de usuário durante o cadastro
O usuário seleciona inicialmente um tipo de usuário.
O sistema apresenta os campos específicos correspondentes ao tipo selecionado.
O usuário altera o tipo de usuário.
O sistema identifica a alteração.
O sistema remove ou oculta os campos específicos do tipo anterior.
O sistema apresenta os campos específicos correspondentes ao novo tipo de usuário.
O usuário preenche os dados necessários.
O fluxo retorna ao passo de confirmação do cadastro.
FA04: Usuário retorna à tela de login
O usuário conclui o cadastro com sucesso.
O sistema apresenta a opção de acessar a tela de login.
O usuário seleciona a opção.
O sistema direciona o usuário para a tela de autenticação.
7. Fluxos de Exceção
EX01: E-mail já cadastrado
O usuário informa um e-mail.
O usuário confirma o cadastro.
O sistema verifica a existência do e-mail no banco de dados.
O sistema identifica que o e-mail já está cadastrado.
O sistema impede a criação de um novo cadastro utilizando o mesmo e-mail.
O sistema informa o usuário sobre a existência do cadastro.

Exemplo de mensagem:

"Este e-mail já está cadastrado. Utilize outro e-mail ou acesse sua conta."

EX02: E-mail inválido
O usuário informa um e-mail em formato inválido.
O sistema realiza a validação.
O sistema identifica que o formato do e-mail é inválido.
O sistema impede o prosseguimento do cadastro.
O sistema solicita a correção do e-mail.

Exemplo de mensagem:

"Informe um endereço de e-mail válido."

EX03: Senhas não coincidem
O usuário informa a senha.
O usuário informa a confirmação da senha.
O sistema compara os dois valores.
O sistema identifica que as senhas são diferentes.
O sistema impede a conclusão do cadastro.
O sistema solicita que o usuário informe novamente as senhas.

Exemplo de mensagem:

"As senhas informadas não coincidem."

EX04: Campo obrigatório não preenchido
O usuário tenta confirmar o cadastro.
O sistema verifica os campos obrigatórios.
O sistema identifica que um ou mais campos não foram preenchidos.
O sistema impede a criação do usuário.
O sistema destaca os campos que precisam ser preenchidos.

Exemplo de mensagem:

"Preencha todos os campos obrigatórios."

EX05: Dados específicos do Lava-jato incompletos
O usuário seleciona o perfil Lava-jato.
O sistema apresenta os campos específicos do estabelecimento.
O usuário deixa um ou mais campos obrigatórios sem preenchimento.
O usuário tenta concluir o cadastro.
O sistema identifica a ausência dos dados obrigatórios.
O sistema impede a criação do cadastro.
O sistema solicita o preenchimento das informações pendentes.
EX06: Falha ao cadastrar usuário
O usuário confirma o cadastro.
O sistema realiza as validações.
O sistema tenta persistir os dados no banco de dados.
Ocorre uma falha durante a operação.
O sistema não conclui o cadastro.
O sistema informa que ocorreu um erro e disponibiliza a opção de tentar novamente.

Exemplo de mensagem:

"Não foi possível realizar o cadastro. Tente novamente."