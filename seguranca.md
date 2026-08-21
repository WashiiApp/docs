# Segurança e Controle de Acesso (Washii)
Este documento detalha a arquitetura de segurança implementada no banco de dados do projeto Washii, utilizando Row Level Security (RLS) do PostgreSQL e controle de privilégios de acesso via GRANT/REVOKE (Princípio do Menor Privilégio).

## 1. Visão Geral da Estratégia
A segurança do Washii opera em duas camadas principais a nível de banco de dados:

**Controle de Acesso por Objeto (GRANT/REVOKE)**: Define quem (papéis/roles do banco) possui permissão para interagir com as tabelas, bloqueando acessos indevidos antes mesmo da consulta ser executada.

**Controle de Acesso por Linha (RLS - Row Level Security)**: Garante o isolamento multi-tenant, permitindo que usuários autenticados (CLIENTE ou LAVA_JATO) visualizem e manipulem apenas os registros que lhes pertencem ou que são de domínio público.

## 2. Gestão de Privilégios (GRANT e REVOKE)
Para evitar exposição de dados sensíveis, o acesso às tabelas transacionais é estritamente controlado entre os papéis anon (usuários não autenticados) e authenticated (usuários logados via Supabase Auth).

```
-- Exemplo de restrição aplicada nas tabelas principais
REVOKE ALL ON TABLE agendamento FROM anon;
GRANT SELECT, INSERT, UPDATE ON TABLE agendamento TO authenticated;
```

### ANON

Usuários não autenticados (`anon`).

Possuem permissão exclusiva de `SELECT` nas tabelas de vitrine e catálogo público:

- `lava_jato`
- `servico` (apenas ativos)
- `categoria_servico`
- `categoria_veiculo`
- `dias_semana`
- `disponibilidade`
- `categoria_veiculo_servico`
- `avaliacao`

### AUTHENTICATED

Usuários autenticados no sistema (`authenticated`).

> **Nota de Arquitetura:** O papel `authenticated` engloba tanto **Clientes** quanto **Lava-Jatos**. Os privilégios de objeto (`GRANT`) abaixo liberam as operações gerais para a role, enquanto a segmentação de quem pode alterar/visualizar cada registro específico é rigorosamente imposta pelas **Políticas de RLS (Row Level Security)**.

#### 1. Tabelas com permissão de `SELECT` (Leitura de Domínio / Apoio)

- `categoria_servico`
- `categoria_veiculo`
- `dias_semana`

#### 2. Tabelas com permissão de `SELECT`, `INSERT` e `UPDATE`

- `usuario`
- `cliente`
- `lava_jato`
- `servico`
- `disponibilidade`
- `veiculo`
- `agendamento`
- `notificacao`

#### 3. Tabelas com permissão de `SELECT`, `INSERT`, `UPDATE` e `DELETE`

> Inclui suporte a **Hard Delete** para itens associativos, exclusões de links e feedback próprio.

- `agendamento_servico`
- `telefone`
- `telefone_usuario`
- `avaliacao`
- `categoria_veiculo_servico`

### RESUMO

| **Tabela** | **Acesso anon (GRANT)** | **Acesso authenticated (GRANT)** |
|---|---|---|
| **`usuario`** | Sem acesso | `SELECT`, `INSERT`, `UPDATE` |
| **`lava_jato`** | `SELECT` | `SELECT`, `INSERT`, `UPDATE` |
| **`cliente`** | Sem acesso | `SELECT`, `INSERT`, `UPDATE` |
| **`servico`** | `SELECT` (Ativos) | `SELECT`, `INSERT`, `UPDATE` |
| **`disponibilidade`** | `SELECT` | `SELECT`, `INSERT`, `UPDATE` |
| **`veiculo`** | Sem acesso | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
| **`agendamento`** | Sem acesso | `SELECT`, `INSERT`, `UPDATE` |
| **`agendamento_servico`** | Sem acesso | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
| **`notificacao`** | Sem acesso | `SELECT`, `INSERT`, `UPDATE` |
| **`telefone`** | Sem acesso | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
| **`telefone_usuario`** | Sem acesso | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
| **`avaliacao`** | `SELECT` | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
| **`categoria_veiculo_servico`** | `SELECT` | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
| **Tabelas de Domínio/Estáticas**  | `SELECT` | `SELECT` |

## 3. Políticas de Segurança por Linha (*Row Level Security - RLS*)

Enquanto o `GRANT` define quais tabelas um papel (*role*) pode acessar, o **RLS** garante o isolamento multi-tenant e a privacidade dos dados. As políticas utilizam a função `auth.uid()` do Supabase para capturar o UUID do usuário autenticado na requisição.

```
-- 1. Habilita o RLS na tabela
ALTER TABLE veiculo ENABLE ROW LEVEL SECURITY;

-- 2. Cria a política unificada de acesso
CREATE POLICY "Exemplo nome" 
-- Tabela em que a regra será aplicada
ON categorias_veiculo 
-- Comando sql que essa política vale (SELECT, INSERT, UPDATE, DELETE, ALL)
FOR SELECT 
-- Quem a regra afeta (anon, authenticated, public)
TO authenticated 
-- filtra quais linhas já existentes podem ser VISUALIZADAS ou MODIFICADAS (SELECT, UPDATE, DELETE)
USING (true);
-- valida os dados nos momentos de INSERT e UPDATE
With check (true) 
```

### A. Módulo de Usuários e Perfis (`usuario`, `cliente`, `lava_jato`)

- **`usuario`:**
  - *SELECT / UPDATE / INSERT:* O usuário só pode visualizar e modificar o seu próprio registro (`id = auth.uid()`).

- **`cliente`:**
  - *SELECT / UPDATE / INSERT:* Restrito estritamente ao próprio perfil do cliente (`id = auth.uid()`).

- **`lava_jato`:**
  - *SELECT:* Público, permitindo que qualquer visitante ou cliente busque estabelecimentos na plataforma.
  - *INSERT / UPDATE:* Restrito exclusivamente ao dono do estabelecimento (`id = auth.uid()`).

### B. Módulo de Operação e Catálogo (`servico`, `disponibilidade`, `categoria_veiculo_servico`)

- **`servico`:**
  - *SELECT:* Público para serviços marcados como ativos (`ativo = true`); visualização completa para o próprio estabelecimento.
  - *INSERT / UPDATE:* Restrito ao lava-jato proprietário do catálogo.

- **`disponibilidade`:**
  - *SELECT:* Público para consulta de horários pelos clientes.
  - *INSERT / UPDATE / DELETE:* Gerenciamento exclusivo do lava-jato dono da agenda.

- **`categoria_veiculo_servico`:**
  - *SELECT:* Público para consulta de preços e durações.
  - *INSERT / UPDATE / DELETE:* Gerenciamento restrito ao lava-jato responsável por associar seus serviços.

### C. Módulo do Cliente (`veiculo`)

- **`veiculo`:**
  - *SELECT / INSERT / UPDATE:* O cliente gerencia exclusivamente os veículos vinculados sob a sua titularidade (`id_cliente = auth.uid()`).

### D. Módulo Transacional (`agendamento`, `agendamento_servico`, `notificacao`)

- **`agendamento`:**
  - *SELECT / INSERT / UPDATE:* Acesso cruzado controlado. O **Cliente** visualiza/gerencia apenas os agendamentos dos seus veículos; o **Lava-Jato** visualiza/gerencia apenas os agendamentos destinados aos seus serviços.

- **`agendamento_servico`:**
  - *SELECT / INSERT / UPDATE / DELETE:* Vinculado diretamente aos agendamentos pertencentes ao escopo do usuário autenticado (cliente ou lava-jato envolvido).

- **`notificacao`:**
  - *SELECT / INSERT / UPDATE:* Restrito estritamente aos usuários envolvidos na transação do agendamento correspondente.

### E. Módulo de Contato (`telefone`, `telefone_usuario`)

- **`telefone` / `telefone_usuario`:**
  - *SELECT / INSERT / UPDATE / DELETE:* O usuário visualiza e gerencia apenas os telefones e associações vinculadas ao seu próprio ID (`id_usuario = auth.uid()`).

### F. Módulo de Avaliações (`avaliacao`)

- **`avaliacao`:**
  - *SELECT:* Público para fins de reputação e vitrine dos estabelecimentos.
  - *INSERT / UPDATE / DELETE:* Restrito exclusivamente ao **Cliente** que realizou o atendimento e é o criador do registro.