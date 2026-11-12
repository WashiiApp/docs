# Contrato Da API

### AuthController (Rota base: `/auth`)

Gerencia a autenticação, registro de novos perfis e o controle de sessão no sistema:

- **Cadastro e Autenticação:**
    - `POST /register/cliente`
      - **Request:** 
      ```` 
        {
          "email": "",
          "senha": "",
          "cidade": "",
          "estado": "",
          "cpf": "",
          "nome": "",
          "sobrenome": ""
        }
      ````
      - **Response:** `201 Created` 
        - `POST /register/lava-jato`
          - **Request:** 
            ````
                {
                  "email": "",
                  "senha": "",
                  "cidade": "",
                  "estado": "",
                  "razaoSocial": "",
                  "nomeFantasia": "",
                  "fluxoSimultaneo": 0,
                  "cnpj": "",
                  "numero": "",
                  "logradouro": "",
                  "bairro": "",
                  "cep": "",
                  "coordenadas": {
                  "latitude": 0.0,
                  "longitude": 0.0
                  }
                }
            ````
            - **Response:** `201 Created`
    - `POST /login`
        - **Request:** `LoginRequest` (JSON contendo email e senha)
        - **Response:** `200 OK` — `LoginResponse` (Contém o token de acesso/JWT e dados básicos do usuário)
- **Gestão de Sessão:**
    - `POST /logout`
        - **Request:** Header `Authorization: Bearer <token>`
        - **Response:** `204 No Content` (Sem corpo)

### ClienteController (Rota base: `/clientes`)

Gerencia as informações cadastrais e a garagem de veículos vinculada ao cliente:

- **Gestão Cadastral do Cliente:**
    - `GET /{id}`
        - **Request:** Path Variable `id` (UUID do cliente)
        - **Response:** `200 OK` — `ClienteResponse` (Dados cadastrais do cliente)
    - `PUT /{id}`
        - **Request:** Path Variable `id` (UUID) + Body `AtualizarClienteRequest` (Novos dados cadastrais)
        - **Response:** `204 No Content` (Sem corpo)
- **Gestão de Veículos:**
    - `POST /{clienteId}/veiculos`
        - **Request:** Path Variable `clienteId` (UUID) + Body `VeiculoDTO` (Marca, modelo, ano, placa, categoriaVeiculoId)
        - **Response:** `201 Created` (Sem corpo)
    - `GET /{clienteId}/veiculos`
        - **Request:** Path Variable `clienteId` (UUID)
        - **Response:** `200 OK` — `List<VeiculoDTO>` (Lista de veículos da garagem)
    - `GET /veiculos/placa/{placa}`
        - **Request:** Path Variable `placa` (String)
        - **Response:** `200 OK` — `VeiculoDTO` (Dados do veículo encontrado)
    - `PUT /{clienteId}/veiculos/{veiculoId}`
        - **Request:** Path Variables `clienteId` e `veiculoId` (UUIDs) + Body `VeiculoDTO` (Dados atualizados)
        - **Response:** `204 No Content` (Sem corpo)
    - `DELETE /{clienteId}/veiculos/{veiculoId}`
        - **Request:** Path Variables `clienteId` e `veiculoId` (UUIDs)
        - **Response:** `204 No Content` (Sem corpo)

### LavaJatoController (Rota base: `/lavajatos`)

Gerencia os dados cadastrais, horários de expediente, catálogo de serviços, avaliações, customização de preços e horários disponíveis do estabelecimento:

- **Gestão Cadastral do Estabelecimento:**
    - `GET /{id}`
        - **Request:** Path Variable `id` (UUID do lava-jato)
        - **Response:** `200 OK` — `LavaJatoResponse` (Dados do estabelecimento, incluindo coordenadas de geolocalização latitude/longitude)
    - `PUT /{id}`
        - **Request:** Path Variable `id` (UUID) + Body `AtualizacaoLavaJatoRequest` (Dados para atualização)
        - **Response:** `204 No Content` (Sem corpo)
    - `DELETE /{id}`
        - **Request:** Path Variable `id` (UUID)
        - **Response:** `204 No Content` (Sem corpo)
- **Expediente:**
    - `POST /{id}/expedientes`
        - **Request:** Path Variable `id` (UUID) + Body `List<ExpedienteRequest>` (Dias da semana, horários de abertura e fechamento)
        - **Response:** `201 Created` — `List<ExpedienteResponse>`
    - `GET /{id}/expedientes`
        - **Request:** Path Variable `id` (UUID)
        - **Response:** `200 OK` — `List<ExpedienteResponse>`
    - `PUT /{id}/expedientes`
        - **Request:** Path Variable `id` (UUID) + Body `List<ExpedienteRequest>`
        - **Response:** `200 OK` — `List<ExpedienteResponse>`
    - `DELETE /{id}/expedientes/{expedienteId}`
        - **Request:** Path Variables `id` e `expedienteId` (UUIDs)
        - **Response:** `204 No Content` (Sem corpo)
- **Serviços:**
    - `POST /{lavaJatoId}/servicos`
        - **Request:** Path Variable `lavaJatoId` (UUID) + Body `CadastroServicoRequest` (Nome, descrição, etc.)
        - **Response:** `204 No Content` (Sem corpo)
    - `GET /{lavaJatoId}/servicos`
        - **Request:** Path Variable `lavaJatoId` (UUID)
        - **Response:** `200 OK` — `List<ServicoResponse>`
    - `GET /{lavaJatoId}/servicos/{servicoId}`
        - **Request:** Path Variables `lavaJatoId` e `servicoId` (UUIDs)
        - **Response:** `200 OK` — `ServicoResponse`
    - `PUT /{lavaJatoId}/servicos/{servicoId}`
        - **Request:** Path Variables `lavaJatoId` e `servicoId` (UUIDs) + Body `CadastroServicoRequest`
        - **Response:** `204 No Content` (Sem corpo)
    - `DELETE /{lavaJatoId}/servicos/{servicoId}`
        - **Request:** Path Variables `lavaJatoId` e `servicoId` (UUIDs)
        - **Response:** `204 No Content` (Sem corpo)
- **Avaliação de Serviços:**
    - `GET /{lavaJatoId}/servicos/{servicoId}/avaliacoes`
        - **Request:** Path Variables `lavaJatoId` e `servicoId` (UUIDs)
        - **Response:** `200 OK` — `List<AvaliacaoResponse>`
    - `POST /{lavaJatoId}/servicos/{servicoId}/avaliacoes`
        - **Request:** Path Variables `lavaJatoId` e `servicoId` (UUIDs) + Body `AvaliacaoRequest` (Nota, comentário)
        - **Response:** `204 No Content` (Sem corpo)
    - `DELETE /{lavaJatoId}/servicos/{servicoId}/avaliacoes/{avaliacaoId}`
        - **Request:** Path Variables `lavaJatoId`, `servicoId` e `avaliacaoId` (UUIDs)
        - **Response:** `204 No Content` (Sem corpo)
- **Preços e Customizações por Categoria de Veículo:**
    - `GET /{lavaJatoId}/servicos/{servicoId}/precos`
        - **Request:** Path Variables `lavaJatoId` e `servicoId` (UUIDs)
        - **Response:** `200 OK` — `List<CategoriaVeiculoServicoResponse>`
    - `POST /{lavaJatoId}/servicos/{servicoId}/precos`
        - **Request:** Path Variables `lavaJatoId` e `servicoId` (UUIDs) + Body `List<CategoriaVeiculoServicoRequest>` (Preço e categoria do veículo)
        - **Response:** `204 No Content` (Sem corpo)
    - `PUT /{lavaJatoId}/servicos/{servicoId}/precos/{precoId}`
        - **Request:** Path Variables `lavaJatoId`, `servicoId` e `precoId` (UUIDs) + Body `CategoriaVeiculoServicoRequest`
        - **Response:** `204 No Content` (Sem corpo)
    - `DELETE /{lavaJatoId}/servicos/{servicoId}/precos/{precoId}`
        - **Request:** Path Variables `lavaJatoId`, `servicoId` e `precoId` (UUIDs)
        - **Response:** `204 No Content` (Sem corpo)
- **Operacionais e Consultas:**
    - `GET /{id}/horarios-disponiveis`
        - **Request:**
            - Path Variable: `id` (UUID do lava-jato)
            - Query Params: `data` (LocalDate, opcional), `servicoIds` (List), `categoriaVeiculoId` (UUID)
        - **Response:** `200 OK` — `HorariosDisponiveisResponse` (Horários livres disponíveis para agendamento)

### AgendamentoController (Rota base: `/agendamentos`)

Gerencia o ciclo de vida dos agendamentos, permitindo criação, cancelamento, alteração de status e filtragem:

- **Operações de Agendamento:**
    - `POST /`
        - **Request:** Header `Authorization: Bearer <token>` + Body `AgendamentoRequest` (LavaJatoId, veiculoId, lista de servicoIds, dataHora)
        - **Response:** `201 Created` — `AgendamentoCriadoResponse`
    - `PATCH /{agendamentoId}/cancelar`
        - **Request:** Header `Authorization: Bearer <token>` + Path Variable `agendamentoId` (UUID)
        - **Response:** `204 No Content` (Sem corpo)
    - `PATCH /{agendamentoId}/status`
        - **Request:** Header `Authorization: Bearer <token>` + Path Variable `agendamentoId` (UUID) + Body `AtualizarStatusAgendamentoRequest` (Novo status)
        - **Response:** `204 No Content` (Sem corpo)
- **Consulta e Filtros:**
    - `GET /{usuarioId}/{agendamentoId}`
        - **Request:** Path Variables `usuarioId` e `agendamentoId` (UUIDs)
        - **Response:** `200 OK` — `AgendamentoCriadoResponse`
    - `GET /{usuarioId}`
        - **Request:**
            - Path Variable: `usuarioId` (UUID)
            - Query Params opcionais: `placaVeiculo` (String), `status` (StatusAgendamento), `dataInicio` (LocalDateTime), `dataFim` (LocalDateTime), `categoriaVeiculoId` (UUID)
        - **Response:** `200 OK` — `List<AgendamentoCriadoResponse>`

### NotificacaoController (Rota base: `/notificacoes`)

Gerencia o histórico unificado de notificações e avisos trocados entre clientes e estabelecimentos:

- **Listagem e Consulta:**
    - `GET /cliente/{clienteId}`
        - **Request:** Path Variable `clienteId` (UUID)
        - **Response:** `200 OK` — `List<NotificacaoResponse>` (Lista de notificações recebidas pelo cliente)
    - `GET /lava-jato/{lavaJatoId}`
        - **Request:** Path Variable `lavaJatoId` (UUID)
        - **Response:** `200 OK` — `List<NotificacaoResponse>` (Lista de notificações recebidas pelo estabelecimento)
- **Gestão de Leitura:**
    - `PATCH /{id}/ler`
        - **Request:** Path Variable `id` (UUID da notificação)
        - **Response:** `204 No Content` (Sem corpo)