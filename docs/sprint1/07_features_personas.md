# FinTwin — Features, Personas e Cenários (Sprint 1)

> Artefactos produzidos no âmbito do Lab#3 — Features, Scenarios, and Stories
> Disciplina: Engenharia de Software 2025/26 — UBI

---

## 1. Personas

As personas representam arquétipos de utilizadores reais do FinTwin, construídas com base nos perfis mais comuns de utilizadores de apps de gestão financeira pessoal.

---

### Persona P01 — João Silva (Estudante Universitário)

| Campo | Detalhe |
|-------|---------|
| **Nome** | João Silva |
| **Idade** | 22 anos |
| **Ocupação** | Estudante de Engenharia Informática, 3.º ano |
| **Rendimento** | 500 EUR/mês (bolsa de estudo + part-time) |
| **Dispositivos** | Smartphone Android, portátil Windows |
| **Objetivo principal** | Perceber para onde vai o dinheiro ao fim do mês |
| **Frustração** | "Gasto mais do que ganho sem perceber onde" |

**Conhecimentos:**
- **User knowledge**: sabe usar apps financeiras básicas
- **Technology knowledge**: alto — é programador
- **Domain knowledge**: baixo — nunca geriu orçamentos formalmente
- **Product knowledge**: nenhum — utilizador novo

**Como usa o FinTwin:**
1. Regista o salário do part-time como receita
2. Importa extrato do banco em CSV mensalmente
3. Consulta o dashboard ao fim do mês
4. Cria orçamento de 100 EUR para restauração

---

### Persona P02 — Maria Santos (Trabalhadora por Conta de Outrem)

| Campo | Detalhe |
|-------|---------|
| **Nome** | Maria Santos |
| **Idade** | 34 anos |
| **Ocupação** | Gestora de Projeto numa empresa de IT |
| **Rendimento** | 2 200 EUR/mês líquidos |
| **Dispositivos** | iPhone, MacBook Pro |
| **Objetivo principal** | Poupar para entrada numa casa (meta: 20 000 EUR em 3 anos) |
| **Frustração** | "Tenho duas contas bancárias e perco o fio ao saldo real" |

**Conhecimentos:**
- **User knowledge**: usa Excel para orçamentos há anos
- **Technology knowledge**: médio — não é técnica mas é fluente digitalmente
- **Domain knowledge**: alto — gere orçamentos profissionalmente
- **Product knowledge**: nenhum — migrou de outra app

**Como usa o FinTwin:**
1. Adiciona as duas contas bancárias (conta corrente + poupança)
2. Regista transações manualmente ao longo do mês
3. Define orçamentos para supermercado, restauração e lazer
4. Consulta o dashboard semanalmente

---

### Persona P03 — Carlos Ferreira (Profissional Liberal)

| Campo | Detalhe |
|-------|---------|
| **Nome** | Carlos Ferreira |
| **Idade** | 47 anos |
| **Ocupação** | Médico com clínica própria |
| **Rendimento** | Variável (3 000–6 000 EUR/mês) |
| **Dispositivos** | iPad, desktop Windows |
| **Objetivo principal** | Separar despesas pessoais das profissionais |
| **Frustração** | "Os meus gastos pessoais e profissionais misturam-se" |

**Conhecimentos:**
- **User knowledge**: utilizador básico de tecnologia
- **Technology knowledge**: baixo
- **Domain knowledge**: alto — tem contabilista mas quer visibilidade própria
- **Product knowledge**: nenhum

**Como usa o FinTwin:**
1. Cria conta separada para despesas pessoais
2. Categoriza despesas manualmente (saúde, transporte, lazer)
3. Exporta relatório mensal em PDF para o contabilista

---

## 2. Features com Template Input / Action / Output

Cada feature do FinTwin é descrita segundo o template:
- **Input**: o que o utilizador fornece ou desencadeia
- **Action**: o que o sistema executa internamente
- **Output**: o que o utilizador recebe/vê como resultado

---

### F01 — Registo de Utilizador

| | |
|-|--|
| **Input** | Nome completo, email, password (obrigatórios); rendimento mensal e moeda (opcionais) |
| **Action** | Sistema valida unicidade do email, faz hash da password (bcrypt, 12 rounds), cria registo na BD, emite JWT |
| **Output** | Conta criada, token JWT devolvido, utilizador redirecionado para wizard de onboarding |

---

### F02 — Login

| | |
|-|--|
| **Input** | Email e password |
| **Action** | Sistema verifica rate limit (max 10 tentativas/min), consulta BD, compara hash da password, gera JWT (expiração 7 dias) |
| **Output** | Token JWT, redirecionamento para dashboard; ou mensagem de erro genérica se credenciais inválidas |

---

### F03 — Gestão de Contas Bancárias (CRUD)

| | |
|-|--|
| **Input** | Nome do banco, tipo de conta (corrente/poupança/investimento), saldo inicial; ou seleção de conta existente para editar/eliminar |
| **Action** | Sistema cria/atualiza/elimina registo na tabela `bank_accounts`; em eliminação, remove transações em cascata (CASCADE) |
| **Output** | Lista de contas atualizada no perfil; saldo total no dashboard recalculado |

---

### F04 — Adicionar Transação Manual

| | |
|-|--|
| **Input** | Descrição, valor (positivo = receita, negativo = despesa), data, conta bancária; categoria (opcional) |
| **Action** | Sistema armazena a transação; se sem categoria, executa categorização automática (match por palavras-chave) |
| **Output** | Transação aparece na lista agrupada por data, dashboard atualizado |

---

### F05 — Importação de Transações via CSV

| | |
|-|--|
| **Input** | Ficheiro CSV com colunas: data, descrição, valor, comerciante; conta bancária de destino |
| **Action** | Sistema faz parse linha a linha (csv.DictReader), categoriza cada transação automaticamente, insere em batch na BD |
| **Output** | Confirmação com número de transações importadas (ex: "47 transações importadas"); lista de transações atualizada |

---

### F06 — Dashboard Financeiro

| | |
|-|--|
| **Input** | Pedido de carregamento da página (autenticado) |
| **Action** | Sistema agrega: soma dos saldos de todas as contas, total de receitas e despesas do mês atual, 5 transações mais recentes, distribuição de despesas por categoria |
| **Output** | Painel com saldo total, resumo mensal (receitas/despesas com variação %), lista de transações recentes, gráfico pie chart por categoria |

---

### F07 — Gestão de Orçamentos Mensais

| | |
|-|--|
| **Input** | Categoria, limite mensal em EUR, período (mês/ano) |
| **Action** | Sistema cria orçamento; ao listar, calcula total gasto nessa categoria no período a partir das transações existentes; calcula percentagem do limite atingido |
| **Output** | Card com barra de progresso colorida (verde < 80%, laranja 80–100%, vermelho > 100%) e valor gasto/limite |

---

### F08 — Categorização Automática

| | |
|-|--|
| **Input** | Descrição e/ou nome do comerciante de uma transação |
| **Action** | Sistema percorre lista de categorias por ordem de prioridade, compara palavras-chave com a descrição (case-insensitive), atribui a primeira categoria com match |
| **Output** | Campo `category_id` da transação preenchido automaticamente; sem match = sem categoria (NULL) |

---

### F09 — Edição de Perfil

| | |
|-|--|
| **Input** | Nome, rendimento mensal, moeda preferida (campos editáveis) |
| **Action** | Sistema atualiza registo na tabela `users`, invalida cache se existir |
| **Output** | Confirmação de alterações guardadas; dados refletidos no dashboard |

---

## 3. Cenários Gherkin (Formato Completo)

Os cenários seguem o formato BDD com as palavras-chave:
`Funcionalidade` / `Cenário` / `Dado` / `Quando` / `Então` / `E` / `Mas`

---

### Funcionalidade: Registo de Utilizador (RF01)

```gherkin
Funcionalidade: Registo de novo utilizador
  Como visitante
  Quero criar uma conta no FinTwin
  Para começar a gerir as minhas finanças pessoais

  Cenário: Registo com dados válidos
    Dado que não tenho conta no sistema
    Quando preencho nome "João Silva", email "joao@email.com" e password "secret123"
    E clico em "Criar Conta"
    Então a conta é criada com sucesso
    E recebo um token JWT
    E sou redirecionado para o wizard de onboarding

  Cenário: Email já registado
    Dado que já existe uma conta com email "joao@email.com"
    Quando tento registar com o mesmo email
    Então vejo a mensagem "Este email já está registado"
    E permaneço na página de registo

  Cenário: Password demasiado curta
    Dado que estou na página de registo
    Quando preencho password com menos de 6 caracteres
    E clico em "Criar Conta"
    Então vejo mensagem "Password demasiado curta"
    E a conta não é criada
```

---

### Funcionalidade: Login (RF02)

```gherkin
Funcionalidade: Autenticação do utilizador
  Como utilizador registado
  Quero fazer login
  Para aceder ao meu dashboard financeiro

  Cenário: Login com credenciais válidas
    Dado que tenho conta com email "joao@email.com" e password "secret123"
    Quando preencho as credenciais corretas
    E clico em "Entrar"
    Então recebo um token JWT
    E sou redirecionado para o dashboard

  Cenário: Password incorreta
    Dado que tenho conta com email "joao@email.com"
    Quando preencho password incorreta "wrongpass"
    Então vejo a mensagem "Email ou password incorretos"
    E não recebo token

  Cenário: Rate limit atingido
    Dado que falhei o login 10 vezes no último minuto
    Quando tento fazer login novamente
    Então vejo a mensagem "Demasiadas tentativas. Tenta mais tarde"
    E o pedido é bloqueado com código 429
```

---

### Funcionalidade: Gestão de Contas Bancárias (RF03)

```gherkin
Funcionalidade: Criar e gerir contas bancárias
  Como utilizador autenticado
  Quero adicionar as minhas contas bancárias
  Para ter uma visão consolidada dos meus saldos

  Cenário: Criar conta bancária com sucesso
    Dado que estou autenticado e na página de perfil
    Quando preencho nome "Caixa Geral", tipo "corrente" e saldo "1500.00"
    E clico em "Adicionar Conta"
    Então a conta aparece na lista de contas
    E o saldo total no dashboard aumenta 1500.00 EUR

  Cenário: Eliminar conta bancária
    Dado que tenho uma conta "NovoBanco" com 3 transações
    Quando clico em "Eliminar" na conta "NovoBanco"
    E confirmo a eliminação
    Então a conta é eliminada
    E as 3 transações associadas são também eliminadas
```

---

### Funcionalidade: Adicionar Transação Manual (RF04)

```gherkin
Funcionalidade: Registar transação manual
  Como utilizador autenticado
  Quero adicionar uma transação manualmente
  Para registar gastos ou receitas que não aparecem no extrato bancário

  Cenário: Adicionar despesa com categoria
    Dado que estou na página de transações
    Quando clico em "Nova Transação"
    E preencho descrição "Almoço", valor "-8.50", data "2026-03-22", categoria "Restauração"
    E clico em "Guardar"
    Então a transação aparece na lista com valor "-8,50 EUR"
    E a categoria "Restauração" está corretamente atribuída

  Cenário: Adicionar transação sem categoria (categorização automática)
    Dado que estou a adicionar uma nova transação
    Quando preencho descrição "Uber Eats" e valor "-12.00" sem selecionar categoria
    Então o sistema atribui automaticamente a categoria "Restauração"
    E a transação aparece categorizada na lista
```

---

### Funcionalidade: Importar CSV (RF05)

```gherkin
Funcionalidade: Importar transações por CSV
  Como utilizador autenticado
  Quero importar um ficheiro CSV do meu banco
  Para adicionar rapidamente um histórico de transações

  Cenário: Importar CSV válido
    Dado que tenho um ficheiro CSV com 47 linhas de transações válidas
    Quando seleciono a conta "Caixa Geral" e faço upload do ficheiro
    E clico em "Confirmar Importação"
    Então vejo "47 transações importadas com sucesso"
    E as transações aparecem na lista agrupadas por data

  Cenário: Ficheiro não é CSV
    Dado que tento fazer upload de um ficheiro .xlsx
    Então vejo a mensagem "Apenas ficheiros CSV são aceites"
    E o upload é cancelado
```

---

### Funcionalidade: Orçamentos Mensais (RF09)

```gherkin
Funcionalidade: Gerir orçamentos por categoria
  Como utilizador autenticado
  Quero definir limites de gasto por categoria
  Para controlar os meus gastos mensais

  Cenário: Criar orçamento mensal
    Dado que estou na página de orçamentos
    Quando clico em "Novo Orçamento"
    E seleciono categoria "Restauração" e defino limite "200"
    E clico em "Guardar"
    Então aparece um card "Restauração" com barra de progresso
    E o limite de 200 EUR está visível

  Cenário: Orçamento excedido (> 100%)
    Dado que o orçamento de "Entretenimento" tem limite 50 EUR
    E já gastei 52 EUR nessa categoria este mês
    Então a barra de progresso aparece a vermelho
    E o valor mostra "52 / 50 EUR (104%)"

  Cenário: Orçamento em zona de aviso (80-100%)
    Dado que o orçamento de "Transportes" tem limite 150 EUR
    E já gastei 130 EUR (87%)
    Então a barra de progresso aparece a laranja
    E vejo o ícone de aviso junto ao orçamento
```
