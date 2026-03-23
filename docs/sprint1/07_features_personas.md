# FinTwin — Features, Personas e Cenarios (Sprint 1)

> Artefactos produzidos no ambito do Lab#3 — Features, Scenarios, and Stories
> Disciplina: Engenharia de Software 2025/26 — UBI

---

## 1. Personas

As personas representam arqutipos de utilizadores reais do FinTwin, construidas com base nos perfis mais comuns de utilizadores de apps de gestao financeira pessoal.

---

### Persona P01 — Joao Silva (Estudante Universitario)

| Campo | Detalhe |
|-------|---------|
| **Nome** | Joao Silva |
| **Idade** | 22 anos |
| **Ocupacao** | Estudante de Engenharia Informatica, 3.º ano |
| **Rendimento** | 500 EUR/mes (bolsa de estudo + part-time) |
| **Dispositivos** | Smartphone Android, portatil Windows |
| **Objetivo principal** | Perceber para onde vai o dinheiro ao fim do mes |
| **Frustracao** | "Gasto mais do que ganho sem perceber onde" |

**Conhecimentos:**
- **User knowledge**: sabe usar apps financeiras basicas
- **Technology knowledge**: alto — e programador
- **Domain knowledge**: baixo — nunca geriu orcamentos formalmente
- **Product knowledge**: nenhum — utilizador novo

**Como usa o FinTwin:**
1. Regista o salario do part-time como receita
2. Importa extrato do banco em CSV mensalmente
3. Consulta o dashboard ao fim do mes
4. Cria orcamento de 100 EUR para restauracao

---

### Persona P02 — Maria Santos (Trabalhadora por Conta de Outrem)

| Campo | Detalhe |
|-------|---------|
| **Nome** | Maria Santos |
| **Idade** | 34 anos |
| **Ocupacao** | Gestora de Projeto numa empresa de IT |
| **Rendimento** | 2 200 EUR/mes liquidos |
| **Dispositivos** | iPhone, MacBook Pro |
| **Objetivo principal** | Poupar para entrada numa casa (meta: 20 000 EUR em 3 anos) |
| **Frustracao** | "Tenho duas contas bancarias e perco o fio ao saldo real" |

**Conhecimentos:**
- **User knowledge**: usa Excel para orcamentos ha anos
- **Technology knowledge**: medio — nao e tecnica mas e fluente digitalmente
- **Domain knowledge**: alto — gere orcamentos profissionalmente
- **Product knowledge**: nenhum — migrou de outra app

**Como usa o FinTwin:**
1. Adiciona as duas contas bancarias (conta corrente + poupanca)
2. Regista transacoes manualmente ao longo do mes
3. Define orcamentos para supermercado, restauracao e lazer
4. Consulta o dashboard semanalmente

---

### Persona P03 — Carlos Ferreira (Profissional Liberal)

| Campo | Detalhe |
|-------|---------|
| **Nome** | Carlos Ferreira |
| **Idade** | 47 anos |
| **Ocupacao** | Medico com clinica propria |
| **Rendimento** | Variavel (3 000–6 000 EUR/mes) |
| **Dispositivos** | iPad, desktop Windows |
| **Objetivo principal** | Separar despesas pessoais das profissionais |
| **Frustracao** | "Os meus gastos pessoais e profissionais misturam-se" |

**Conhecimentos:**
- **User knowledge**: utilizador basico de tecnologia
- **Technology knowledge**: baixo
- **Domain knowledge**: alto — tem contabilista mas quer visibilidade propria
- **Product knowledge**: nenhum

**Como usa o FinTwin:**
1. Cria conta separada para despesas pessoais
2. Categoriza despesas manualmente (saude, transporte, lazer)
3. Exporta relatorio mensal em PDF para o contabilista

---

## 2. Features com Template Input / Action / Output

Cada feature do FinTwin e descrita segundo o template:
- **Input**: o que o utilizador fornece ou desencadeia
- **Action**: o que o sistema executa internamente
- **Output**: o que o utilizador recebe/ve como resultado

---

### F01 — Registo de Utilizador

| | |
|-|--|
| **Input** | Nome completo, email, password (obrigatorios); rendimento mensal e moeda (opcionais) |
| **Action** | Sistema valida unicidade do email, faz hash da password (bcrypt, 12 rounds), cria registo na BD, emite JWT |
| **Output** | Conta criada, token JWT devolvido, utilizador redirecionado para wizard de onboarding |

---

### F02 — Login

| | |
|-|--|
| **Input** | Email e password |
| **Action** | Sistema verifica rate limit (max 10 tentativas/min), consulta BD, compara hash da password, gera JWT (expiracao 7 dias) |
| **Output** | Token JWT, redirecionamento para dashboard; ou mensagem de erro generica se credenciais invalidas |

---

### F03 — Gestao de Contas Bancarias (CRUD)

| | |
|-|--|
| **Input** | Nome do banco, tipo de conta (corrente/poupanca/investimento), saldo inicial; ou selecao de conta existente para editar/eliminar |
| **Action** | Sistema cria/atualiza/elimina registo na tabela `bank_accounts`; em eliminacao, remove transacoes em cascata (CASCADE) |
| **Output** | Lista de contas atualizada no perfil; saldo total no dashboard recalculado |

---

### F04 — Adicionar Transacao Manual

| | |
|-|--|
| **Input** | Descricao, valor (positivo = receita, negativo = despesa), data, conta bancaria; categoria (opcional) |
| **Action** | Sistema armazena a transacao; se sem categoria, executa categorizacao automatica (match por palavras-chave) |
| **Output** | Transacao aparece na lista agrupada por data, dashboard atualizado |

---

### F05 — Importacao de Transacoes via CSV

| | |
|-|--|
| **Input** | Ficheiro CSV com colunas: data, descricao, valor, comerciante; conta bancaria de destino |
| **Action** | Sistema faz parse linha a linha (csv.DictReader), categoriza cada transacao automaticamente, insere em batch na BD |
| **Output** | Confirmacao com numero de transacoes importadas (ex: "47 transacoes importadas"); lista de transacoes atualizada |

---

### F06 — Dashboard Financeiro

| | |
|-|--|
| **Input** | Pedido de carregamento da pagina (autenticado) |
| **Action** | Sistema agrega: soma dos saldos de todas as contas, total de receitas e despesas do mes atual, 5 transacoes mais recentes, distribuicao de despesas por categoria |
| **Output** | Painel com saldo total, resumo mensal (receitas/despesas com variacao %), lista de transacoes recentes, grafico pie chart por categoria |

---

### F07 — Gestao de Orcamentos Mensais

| | |
|-|--|
| **Input** | Categoria, limite mensal em EUR, periodo (mes/ano) |
| **Action** | Sistema cria orcamento; ao listar, calcula total gasto nessa categoria no periodo a partir das transacoes existentes; calcula percentagem do limite atingido |
| **Output** | Card com barra de progresso colorida (verde < 80%, laranja 80–100%, vermelho > 100%) e valor gasto/limite |

---

### F08 — Categorizacao Automatica

| | |
|-|--|
| **Input** | Descricao e/ou nome do comerciante de uma transacao |
| **Action** | Sistema percorre lista de categorias por ordem de prioridade, compara palavras-chave com a descricao (case-insensitive), atribui a primeira categoria com match |
| **Output** | Campo `category_id` da transacao preenchido automaticamente; sem match = sem categoria (NULL) |

---

### F09 — Edicao de Perfil

| | |
|-|--|
| **Input** | Nome, rendimento mensal, moeda preferida (campos editaveis) |
| **Action** | Sistema atualiza registo na tabela `users`, invalida cache se existir |
| **Output** | Confirmacao de alteracoes guardadas; dados refletidos no dashboard |

---

## 3. Cenarios Gherkin (Formato Completo)

Os cenarios seguem o formato BDD com as palavras-chave:
`Funcionalidade` / `Cenario` / `Dado` / `Quando` / `Entao` / `E` / `Mas`

---

### Funcionalidade: Registo de Utilizador (RF01)

```gherkin
Funcionalidade: Registo de novo utilizador
  Como visitante
  Quero criar uma conta no FinTwin
  Para comecar a gerir as minhas financas pessoais

  Cenario: Registo com dados validos
    Dado que nao tenho conta no sistema
    Quando preencho nome "Joao Silva", email "joao@email.com" e password "secret123"
    E clico em "Criar Conta"
    Entao a conta e criada com sucesso
    E recebo um token JWT
    E sou redirecionado para o wizard de onboarding

  Cenario: Email ja registado
    Dado que ja existe uma conta com email "joao@email.com"
    Quando tento registar com o mesmo email
    Entao vejo a mensagem "Este email ja esta registado"
    E permaneco na pagina de registo

  Cenario: Password demasiado curta
    Dado que estou na pagina de registo
    Quando preencho password com menos de 6 caracteres
    E clico em "Criar Conta"
    Entao vejo mensagem "Password demasiado curta"
    E a conta nao e criada
```

---

### Funcionalidade: Login (RF02)

```gherkin
Funcionalidade: Autenticacao do utilizador
  Como utilizador registado
  Quero fazer login
  Para aceder ao meu dashboard financeiro

  Cenario: Login com credenciais validas
    Dado que tenho conta com email "joao@email.com" e password "secret123"
    Quando preencho as credenciais corretas
    E clico em "Entrar"
    Entao recebo um token JWT
    E sou redirecionado para o dashboard

  Cenario: Password incorreta
    Dado que tenho conta com email "joao@email.com"
    Quando preencho password incorreta "wrongpass"
    Entao vejo a mensagem "Email ou password incorretos"
    E nao recebo token

  Cenario: Rate limit atingido
    Dado que falhei o login 10 vezes no ultimo minuto
    Quando tento fazer login novamente
    Entao vejo a mensagem "Demasiadas tentativas. Tenta mais tarde"
    E o pedido e bloqueado com codigo 429
```

---

### Funcionalidade: Gestao de Contas Bancarias (RF03)

```gherkin
Funcionalidade: Criar e gerir contas bancarias
  Como utilizador autenticado
  Quero adicionar as minhas contas bancarias
  Para ter uma visao consolidada dos meus saldos

  Cenario: Criar conta bancaria com sucesso
    Dado que estou autenticado e na pagina de perfil
    Quando preencho nome "Caixa Geral", tipo "corrente" e saldo "1500.00"
    E clico em "Adicionar Conta"
    Entao a conta aparece na lista de contas
    E o saldo total no dashboard aumenta 1500.00 EUR

  Cenario: Eliminar conta bancaria
    Dado que tenho uma conta "NovoBanco" com 3 transacoes
    Quando clico em "Eliminar" na conta "NovoBanco"
    E confirmo a eliminacao
    Entao a conta e eliminada
    E as 3 transacoes associadas sao tambem eliminadas
```

---

### Funcionalidade: Adicionar Transacao Manual (RF04)

```gherkin
Funcionalidade: Registar transacao manual
  Como utilizador autenticado
  Quero adicionar uma transacao manualmente
  Para registar gastos ou receitas que nao aparecem no extrato bancario

  Cenario: Adicionar despesa com categoria
    Dado que estou na pagina de transacoes
    Quando clico em "Nova Transacao"
    E preencho descricao "Almoco", valor "-8.50", data "2026-03-22", categoria "Restauracao"
    E clico em "Guardar"
    Entao a transacao aparece na lista com valor "-8,50 EUR"
    E a categoria "Restauracao" esta corretamente atribuida

  Cenario: Adicionar transacao sem categoria (categorizacao automatica)
    Dado que estou a adicionar uma nova transacao
    Quando preencho descricao "Uber Eats" e valor "-12.00" sem selecionar categoria
    Entao o sistema atribui automaticamente a categoria "Restauracao"
    E a transacao aparece categorizada na lista
```

---

### Funcionalidade: Importar CSV (RF05)

```gherkin
Funcionalidade: Importar transacoes por CSV
  Como utilizador autenticado
  Quero importar um ficheiro CSV do meu banco
  Para adicionar rapidamente um historico de transacoes

  Cenario: Importar CSV valido
    Dado que tenho um ficheiro CSV com 47 linhas de transacoes validas
    Quando seleciono a conta "Caixa Geral" e faço upload do ficheiro
    E clico em "Confirmar Importacao"
    Entao vejo "47 transacoes importadas com sucesso"
    E as transacoes aparecem na lista agrupadas por data

  Cenario: Ficheiro nao e CSV
    Dado que tento fazer upload de um ficheiro .xlsx
    Entao vejo a mensagem "Apenas ficheiros CSV sao aceites"
    E o upload e cancelado
```

---

### Funcionalidade: Orcamentos Mensais (RF09)

```gherkin
Funcionalidade: Gerir orcamentos por categoria
  Como utilizador autenticado
  Quero definir limites de gasto por categoria
  Para controlar os meus gastos mensais

  Cenario: Criar orcamento mensal
    Dado que estou na pagina de orcamentos
    Quando clico em "Novo Orcamento"
    E seleciono categoria "Restauracao" e defino limite "200"
    E clico em "Guardar"
    Entao aparece um card "Restauracao" com barra de progresso
    E o limite de 200 EUR esta visivel

  Cenario: Orcamento excedido (> 100%)
    Dado que o orcamento de "Entretenimento" tem limite 50 EUR
    E ja gastei 52 EUR nessa categoria este mes
    Entao a barra de progresso aparece a vermelho
    E o valor mostra "52 / 50 EUR (104%)"

  Cenario: Orcamento em zona de aviso (80-100%)
    Dado que o orcamento de "Transportes" tem limite 150 EUR
    E ja gastei 130 EUR (87%)
    Entao a barra de progresso aparece a laranja
    E vejo o icone de aviso junto ao orcamento
```
