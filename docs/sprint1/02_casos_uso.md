# FinTwin — Casos de Uso (Sprint 1)

## 1. Diagrama de Casos de Uso

```mermaid
graph TB
    Actor((Utilizador))

    subgraph Autenticacao
        UC01[UC01: Registar Conta]
        UC02[UC02: Fazer Login]
        UC03[UC03: Fazer Logout]
    end

    subgraph Contas Bancarias
        UC04[UC04: Criar Conta Bancaria]
        UC05[UC05: Listar Contas]
        UC06[UC06: Editar Conta]
        UC07[UC07: Eliminar Conta]
    end

    subgraph Transacoes
        UC08[UC08: Adicionar Transacao]
        UC09[UC09: Listar Transacoes]
        UC10[UC10: Importar CSV]
        UC11[UC11: Editar Transacao]
    end

    subgraph Orcamentos
        UC12[UC12: Criar Orcamento]
        UC13[UC13: Ver Resumo Orcamental]
        UC14[UC14: Eliminar Orcamento]
    end

    subgraph Dashboard
        UC15[UC15: Ver Dashboard]
    end

    subgraph Perfil
        UC16[UC16: Editar Perfil]
    end

    Actor --> UC01
    Actor --> UC02
    Actor --> UC03
    Actor --> UC04
    Actor --> UC05
    Actor --> UC06
    Actor --> UC07
    Actor --> UC08
    Actor --> UC09
    Actor --> UC10
    Actor --> UC11
    Actor --> UC12
    Actor --> UC13
    Actor --> UC14
    Actor --> UC15
    Actor --> UC16
```

---

## 2. Descricoes Detalhadas

---

### UC01 — Registar Conta

| Campo | Descricao |
|-------|-----------|
| **Ator** | Utilizador nao autenticado |
| **Pre-condicao** | O utilizador nao possui conta no sistema |
| **Pos-condicao** | Conta criada na base de dados; JWT emitido; utilizador redirecionado para o onboarding |
| **Requisito** | RF01 |

**Fluxo Principal:**
1. O utilizador acede a pagina de registo (`/register`)
2. Preenche os campos: nome completo, email, password
3. Opcionalmente preenche: rendimento mensal, moeda preferida
4. Clica em "Criar Conta"
5. O sistema valida os dados (email formato valido, password minimo 6 caracteres)
6. O sistema verifica que o email nao esta registado
7. O sistema cria a conta com password encriptada (bcrypt, 12 rounds)
8. O sistema emite um JWT e armazena-o no localStorage
9. O utilizador e redirecionado para o wizard de onboarding

**Fluxos Alternativos:**

| ID | Condicao | Acao |
|----|----------|------|
| 5a | Email com formato invalido | Sistema exibe "Email invalido" |
| 5b | Password com menos de 6 caracteres | Sistema exibe "Password demasiado curta" |
| 6a | Email ja registado | Sistema exibe "Este email ja esta registado" (HTTP 409) |

---

### UC02 — Fazer Login

| Campo | Descricao |
|-------|-----------|
| **Ator** | Utilizador registado, nao autenticado |
| **Pre-condicao** | O utilizador possui conta ativa no sistema |
| **Pos-condicao** | JWT armazenado no localStorage; utilizador redirecionado para o dashboard |
| **Requisito** | RF02 |

**Fluxo Principal:**
1. O utilizador acede a pagina de login (`/login`)
2. Preenche email e password
3. Clica em "Entrar"
4. O sistema verifica as credenciais na base de dados
5. O sistema gera um JWT com o ID do utilizador
6. O token e armazenado no localStorage do browser
7. O utilizador e redirecionado para o dashboard (`/`)

**Fluxos Alternativos:**

| ID | Condicao | Acao |
|----|----------|------|
| 4a | Email nao encontrado | Sistema exibe "Email ou password incorretos" (HTTP 401) |
| 4b | Password incorreta | Sistema exibe "Email ou password incorretos" (HTTP 401) |
| 4c | Rate limit excedido (>10 tentativas/min) | Sistema exibe "Demasiadas tentativas, tenta novamente mais tarde" (HTTP 429) |

**Nota de seguranca:** A mensagem de erro e generica para nao revelar se o email existe no sistema.

---

### UC03 — Fazer Logout

| Campo | Descricao |
|-------|-----------|
| **Ator** | Utilizador autenticado |
| **Pre-condicao** | JWT valido no localStorage |
| **Pos-condicao** | JWT removido; utilizador redirecionado para login |
| **Requisito** | RF02 |

**Fluxo Principal:**
1. O utilizador clica no botao "Sair" na sidebar
2. O sistema remove o JWT do localStorage
3. O utilizador e redirecionado para a pagina de login (`/login`)

---

### UC04 — Criar Conta Bancaria

| Campo | Descricao |
|-------|-----------|
| **Ator** | Utilizador autenticado |
| **Pre-condicao** | Utilizador com sessao ativa |
| **Pos-condicao** | Nova conta bancaria criada e associada ao utilizador |
| **Requisito** | RF03 |

**Fluxo Principal:**
1. O utilizador navega para o perfil ou clica em "Nova Conta" no dashboard
2. O modal de criacao de conta abre
3. O utilizador preenche: nome do banco, tipo de conta (corrente/poupanca/investimento), saldo atual
4. Clica em "Criar Conta"
5. O sistema valida os dados (nome obrigatorio)
6. O sistema cria a conta bancaria na base de dados
7. O modal fecha e a lista de contas e atualizada

**Fluxos Alternativos:**

| ID | Condicao | Acao |
|----|----------|------|
| 5a | Nome do banco vazio | Sistema exibe "Nome do banco e obrigatorio" |
| 3a | Utilizador clica "Cancelar" | Modal fecha sem criar conta |

---

### UC05 — Listar Contas

| Campo | Descricao |
|-------|-----------|
| **Ator** | Utilizador autenticado |
| **Pre-condicao** | Utilizador com sessao ativa |
| **Pos-condicao** | Lista de contas bancarias apresentada |
| **Requisito** | RF03 |

**Fluxo Principal:**
1. O utilizador acede a pagina de perfil (`/profile`)
2. O sistema carrega as contas bancarias do utilizador via API
3. As contas sao apresentadas em cards com: nome do banco, tipo, saldo, moeda

**Fluxos Alternativos:**

| ID | Condicao | Acao |
|----|----------|------|
| 3a | Sem contas registadas | Sistema exibe mensagem "Ainda nao tens contas bancarias" com CTA para criar |

---

### UC08 — Adicionar Transacao

| Campo | Descricao |
|-------|-----------|
| **Ator** | Utilizador autenticado |
| **Pre-condicao** | Utilizador tem pelo menos uma conta bancaria |
| **Pos-condicao** | Transacao criada e categorizada; cache de scores invalidado |
| **Requisito** | RF04 |

**Fluxo Principal:**
1. O utilizador clica em "Nova Transacao" na pagina de transacoes
2. O modal de adicionar transacao abre
3. O utilizador preenche: descricao, valor (negativo = despesa), data, conta de destino
4. Opcionalmente seleciona uma categoria
5. Clica em "Adicionar"
6. O sistema valida os dados
7. Se nenhuma categoria foi selecionada, o sistema executa a categorizacao automatica com base na descricao
8. A transacao e guardada na base de dados
9. O modal fecha e a lista de transacoes e atualizada

**Fluxos Alternativos:**

| ID | Condicao | Acao |
|----|----------|------|
| 6a | Descricao ou valor vazio | Sistema exibe mensagem de validacao |
| 4a | Utilizador nao tem contas | Botao desativado; mensagem "Cria uma conta primeiro" |
| 7a | Categorizacao automatica sem resultado | Transacao guardada sem categoria (category_id = NULL) |

---

### UC09 — Listar Transacoes

| Campo | Descricao |
|-------|-----------|
| **Ator** | Utilizador autenticado |
| **Pre-condicao** | Sessao ativa |
| **Pos-condicao** | Lista paginada de transacoes apresentada |
| **Requisito** | RF06 |

**Fluxo Principal:**
1. O utilizador navega para a pagina de transacoes (`/transactions`)
2. O sistema carrega as transacoes mais recentes (limite: 50, offset: 0)
3. As transacoes sao apresentadas agrupadas por data
4. Cada transacao mostra: descricao, valor, categoria (com icone e cor), data, comerciante
5. O utilizador pode filtrar por: tipo (receita/despesa), categoria, periodo

**Fluxos Alternativos:**

| ID | Condicao | Acao |
|----|----------|------|
| 3a | Sem transacoes | Estado vazio com mensagem e CTA para adicionar |
| 5a | Filtro sem resultados | Mensagem "Nenhuma transacao encontrada com estes filtros" |

---

### UC10 — Importar CSV

| Campo | Descricao |
|-------|-----------|
| **Ator** | Utilizador autenticado |
| **Pre-condicao** | Pelo menos uma conta bancaria; ficheiro CSV no formato correto |
| **Pos-condicao** | N transacoes importadas e categorizadas automaticamente |
| **Requisito** | RF05 |

**Fluxo Principal:**
1. O utilizador clica em "Importar CSV" na pagina de transacoes
2. O modal de importacao abre
3. O utilizador seleciona a conta de destino
4. Arrasta ou seleciona o ficheiro CSV
5. O sistema le o ficheiro e apresenta pre-visualizacao das primeiras 5 linhas
6. O utilizador ve o numero total de linhas detetadas
7. Clica em "Confirmar Import"
8. O sistema processa cada linha: parse da data, descricao, valor e comerciante
9. Cada transacao e categorizada automaticamente
10. O sistema retorna o numero de transacoes importadas com sucesso
11. O modal fecha e a lista de transacoes e atualizada

**Fluxos Alternativos:**

| ID | Condicao | Acao |
|----|----------|------|
| 4a | Ficheiro nao e CSV | Sistema exibe "Apenas ficheiros CSV sao aceites" |
| 8a | Formato de data invalido | Transacao ignorada; erro reportado |
| 8b | Colunas em falta | Sistema exibe "Erro ao importar. Verifica o formato do ficheiro" |

**Formato CSV esperado:**
```
date,description,amount,merchant
2026-01-15,Supermercado Continente,-45.30,Continente
2026-01-16,Salario,1500.00,Empresa XYZ
```

---

### UC12 — Criar Orcamento

| Campo | Descricao |
|-------|-----------|
| **Ator** | Utilizador autenticado |
| **Pre-condicao** | Existem categorias no sistema |
| **Pos-condicao** | Orcamento mensal criado para a categoria selecionada |
| **Requisito** | RF09 |

**Fluxo Principal:**
1. O utilizador navega para a pagina de orcamentos (`/budgets`)
2. Clica em "Novo Orcamento"
3. Seleciona a categoria (ex: Restauracao, Transportes)
4. Define o limite mensal (ex: 200 EUR)
5. O sistema define automaticamente o periodo (mes atual ate fim do ano)
6. Clica em "Criar"
7. O sistema guarda o orcamento e apresenta o card com barra de progresso

**Fluxos Alternativos:**

| ID | Condicao | Acao |
|----|----------|------|
| 3a | Nenhuma categoria disponivel | Mensagem de erro |
| 4a | Limite inferior a 1 EUR | Validacao impede a criacao |

---

### UC15 — Ver Dashboard

| Campo | Descricao |
|-------|-----------|
| **Ator** | Utilizador autenticado |
| **Pre-condicao** | Sessao ativa |
| **Pos-condicao** | Dashboard apresentado com dados atualizados |
| **Requisito** | RF07 |

**Fluxo Principal:**
1. O utilizador acede ao dashboard (`/`) apos login
2. O sistema carrega em paralelo: contas bancarias, transacoes recentes, resumo mensal
3. O dashboard apresenta:
   - Saldo total (soma de todas as contas)
   - Receitas e despesas do mes atual
   - Ultimas transacoes (5-10 mais recentes)
   - Grafico de distribuicao de despesas por categoria
4. Se o utilizador e novo (sem contas), apresenta o wizard de onboarding

**Fluxos Alternativos:**

| ID | Condicao | Acao |
|----|----------|------|
| 4a | Utilizador novo | Wizard de onboarding e apresentado (4 passos) |
| 2a | Erro ao carregar dados | Mensagem de erro com botao "Tentar novamente" |

---

### UC16 — Editar Perfil

| Campo | Descricao |
|-------|-----------|
| **Ator** | Utilizador autenticado |
| **Pre-condicao** | Sessao ativa |
| **Pos-condicao** | Dados do perfil atualizados |
| **Requisito** | RF10 |

**Fluxo Principal:**
1. O utilizador navega para o perfil (`/profile`)
2. O sistema apresenta os dados atuais: nome, email, rendimento mensal, moeda
3. O utilizador edita os campos desejados
4. Clica em "Guardar Alteracoes"
5. O sistema valida e atualiza os dados via PUT /api/v1/auth/me
6. Confirmacao visual de sucesso (toast notification)

**Fluxos Alternativos:**

| ID | Condicao | Acao |
|----|----------|------|
| 5a | Dados invalidos | Mensagem de validacao nos campos afetados |

---

## 3. Matriz Casos de Uso vs Requisitos Funcionais

| Caso de Uso | RF01 | RF02 | RF03 | RF04 | RF05 | RF06 | RF07 | RF08 | RF09 | RF10 |
|-------------|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| UC01 Registar | X | | | | | | | | | |
| UC02 Login | | X | | | | | | | | |
| UC03 Logout | | X | | | | | | | | |
| UC04 Criar Conta | | | X | | | | | | | |
| UC05 Listar Contas | | | X | | | | | | | |
| UC06 Editar Conta | | | X | | | | | | | |
| UC07 Eliminar Conta | | | X | | | | | | | |
| UC08 Adicionar Txn | | | | X | | | | X | | |
| UC09 Listar Txn | | | | | | X | | | | |
| UC10 Importar CSV | | | | | X | | | X | | |
| UC11 Editar Txn | | | | X | | | | | | |
| UC12 Criar Orcamento | | | | | | | | | X | |
| UC13 Ver Resumo Orc. | | | | | | | | | X | |
| UC14 Eliminar Orc. | | | | | | | | | X | |
| UC15 Ver Dashboard | | | | | | | X | | | |
| UC16 Editar Perfil | | | | | | | | | | X |
