# FinTwin — Diagramas de Sequencia (Sprint 1)

## DS01 — Login do Utilizador

```mermaid
sequenceDiagram
    actor U as Utilizador
    participant FE as Frontend (React)
    participant API as Backend (FastAPI)
    participant DB as PostgreSQL

    U->>FE: Preenche email e password
    U->>FE: Clica "Entrar"
    FE->>API: POST /api/v1/auth/login {email, password}

    API->>API: Rate limit check (10/min)

    alt Rate limit excedido
        API-->>FE: 429 Too Many Requests
        FE-->>U: "Demasiadas tentativas"
    end

    API->>DB: SELECT * FROM users WHERE email = ?
    DB-->>API: user record (ou null)

    alt Email nao encontrado
        API-->>FE: 401 "Email ou password incorretos"
        FE-->>U: Mensagem de erro
    end

    API->>API: bcrypt.verify(password, user.password_hash)

    alt Password incorreta
        API-->>FE: 401 "Email ou password incorretos"
        FE-->>U: Mensagem de erro
    end

    API->>API: create_access_token(user_id)
    API-->>FE: 200 {access_token: "JWT..."}
    FE->>FE: localStorage.setItem("token", jwt)
    FE-->>U: Redireciona para Dashboard (/)
```

---

## DS02 — Criar Transacao

```mermaid
sequenceDiagram
    actor U as Utilizador
    participant FE as Frontend (React)
    participant API as Backend (FastAPI)
    participant CAT as Categorizer Service
    participant DB as PostgreSQL
    participant RD as Redis

    U->>FE: Preenche descricao, valor, data
    U->>FE: Clica "Adicionar"
    FE->>API: POST /api/v1/transactions {account_id, description, amount, date}
    Note over FE,API: Header: Authorization: Bearer JWT

    API->>API: Decode JWT, extrair user_id
    API->>DB: SELECT bank_account WHERE id=? AND user_id=?
    DB-->>API: account (ou null)

    alt Conta nao encontrada
        API-->>FE: 404 "Conta nao encontrada"
        FE-->>U: Mensagem de erro
    end

    alt Sem categoria selecionada
        API->>CAT: categorize_transaction(description, merchant)
        CAT-->>API: (category_name, confidence)
        API->>DB: SELECT category WHERE name = ?
        DB-->>API: category_id
    end

    API->>DB: INSERT INTO transactions (...)
    DB-->>API: transaction record
    API->>RD: cache_invalidate("score:{user_id}:*")
    API-->>FE: 201 {id, amount, description, category, ...}
    FE->>FE: Atualizar lista de transacoes
    FE-->>U: Modal fecha, transacao visivel na lista
```

---

## DS03 — Importar CSV

```mermaid
sequenceDiagram
    actor U as Utilizador
    participant FE as Frontend (React)
    participant API as Backend (FastAPI)
    participant CAT as Categorizer Service
    participant DB as PostgreSQL

    U->>FE: Clica "Importar CSV"
    FE-->>U: Modal de importacao abre

    U->>FE: Seleciona conta de destino
    U->>FE: Arrasta ficheiro CSV

    FE->>FE: FileReader.readAsText(file)
    FE->>FE: parseCsvPreview(text)
    FE-->>U: Pre-visualizacao (5 primeiras linhas) + total

    U->>FE: Clica "Confirmar Import"
    FE->>API: POST /api/v1/transactions/upload-csv (multipart: account_id + file)
    Note over FE,API: Header: Authorization: Bearer JWT

    API->>DB: Verificar account pertence ao user
    API->>API: csv.DictReader(file)

    loop Para cada linha do CSV
        API->>CAT: categorize_transaction(description, merchant)
        CAT-->>API: (category_name, confidence)
        API->>DB: Resolver category_id pelo nome
        API->>DB: INSERT INTO transactions (source=CSV, ...)
    end

    API->>DB: flush() — commit batch
    API->>API: cache_invalidate scores
    API-->>FE: 200 {imported: N, message: "N transacoes importadas"}
    FE-->>U: Modal fecha, lista atualizada
```

---

## DS04 — Ver Dashboard

```mermaid
sequenceDiagram
    actor U as Utilizador
    participant FE as Frontend (React)
    participant API as Backend (FastAPI)
    participant DB as PostgreSQL

    U->>FE: Acede ao Dashboard (/)
    FE->>FE: Verificar token JWT no localStorage

    alt Token ausente ou expirado
        FE-->>U: Redireciona para /login
    end

    par Chamadas em paralelo
        FE->>API: GET /api/v1/auth/me
        API->>DB: SELECT user
        DB-->>API: user data
        API-->>FE: {full_name, email, currency}
    and
        FE->>API: GET /api/v1/bank-accounts
        API->>DB: SELECT bank_accounts WHERE user_id = ?
        DB-->>API: accounts[]
        API-->>FE: [{bank_name, balance, type}, ...]
    and
        FE->>API: GET /api/v1/transactions?limit=10
        API->>DB: SELECT transactions ORDER BY date DESC LIMIT 10
        DB-->>API: transactions[]
        API-->>FE: [{description, amount, category, date}, ...]
    and
        FE->>API: GET /api/v1/transactions/summary?month=3&year=2026
        API->>DB: Aggregate by category for month
        DB-->>API: summary data
        API-->>FE: {total_income, total_expenses, net, by_category}
    end

    FE->>FE: Calcular saldo total (soma dos saldos das contas)
    FE->>FE: Renderizar: hero number, stat cards, transacoes, grafico
    FE-->>U: Dashboard completo apresentado
```

---

## DS05 — Criar Orcamento

```mermaid
sequenceDiagram
    actor U as Utilizador
    participant FE as Frontend (React)
    participant API as Backend (FastAPI)
    participant DB as PostgreSQL

    U->>FE: Clica "Novo Orcamento"
    FE-->>U: Formulario inline abre

    U->>FE: Seleciona categoria "Restauracao"
    U->>FE: Define limite "200" EUR
    U->>FE: Clica "Criar"

    FE->>API: POST /api/v1/budgets {category_id, monthly_limit, period_start, period_end}
    Note over FE,API: Header: Authorization: Bearer JWT

    API->>API: Decode JWT, extrair user_id
    API->>DB: INSERT INTO budgets (user_id, category_id, monthly_limit, ...)
    DB-->>API: budget record
    API-->>FE: 201 {id, category_id, monthly_limit, period_start, period_end}

    FE->>API: GET /api/v1/budgets/summary?month=3&year=2026
    API->>DB: Aggregate spending per budget category
    DB-->>API: [{category, spent, limit, percentage}, ...]
    API-->>FE: Summary data

    FE->>FE: Renderizar card com barra de progresso
    FE-->>U: Orcamento visivel com 0% gasto
```
