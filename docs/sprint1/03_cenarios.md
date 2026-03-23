# FinTwin — Cenários e User Stories (Sprint 1)

## Formato

Cada user story segue o formato:
- **Como** [ator], **quero** [ação], **para** [benefício]
- Cenários no formato Gherkin: **Dado** / **Quando** / **Então**

---

## US01 — Registo de Utilizador

> Como visitante, quero criar uma conta no FinTwin, para começar a gerir as minhas finanças.

### Cenário 1.1: Registo com sucesso
```
Dado que não tenho conta no sistema
Quando preencho nome "João Silva", email "joao@email.com" e password "secret123"
E clico em "Criar Conta"
Então a conta é criada com sucesso
E recebo um token JWT
E sou redirecionado para o wizard de onboarding
```

### Cenário 1.2: Email já registado
```
Dado que já existe uma conta com email "joao@email.com"
Quando tento registar com o mesmo email
Então vejo a mensagem "Este email já está registado"
E permaneço na página de registo
```

### Cenário 1.3: Password demasiado curta
```
Dado que estou na página de registo
Quando preencho uma password com menos de 6 caracteres
E clico em "Criar Conta"
Então vejo a mensagem de validação "Password demasiado curta"
E a conta não é criada
```

---

## US02 — Login

> Como utilizador registado, quero fazer login, para aceder ao meu dashboard financeiro.

### Cenário 2.1: Login com sucesso
```
Dado que tenho conta com email "joao@email.com" e password "secret123"
Quando preencho as credenciais corretas
E clico em "Entrar"
Então recebo um token JWT
E sou redirecionado para o dashboard
```

### Cenário 2.2: Credenciais incorretas
```
Dado que tenho conta com email "joao@email.com"
Quando preencho password incorreta
E clico em "Entrar"
Então vejo a mensagem "Email ou password incorretos"
E permaneço na página de login
```

### Cenário 2.3: Rate limit excedido
```
Dado que fiz 10 tentativas de login no último minuto
Quando tento fazer login novamente
Então recebo resposta HTTP 429
E vejo mensagem "Demasiadas tentativas, tenta novamente mais tarde"
```

---

## US03 — Logout

> Como utilizador autenticado, quero fazer logout, para terminar a minha sessão de forma segura.

### Cenário 3.1: Logout com sucesso
```
Dado que estou autenticado no sistema
Quando clico no botão "Sair" na sidebar
Então o token JWT é removido do localStorage
E sou redirecionado para a página de login
```

---

## US04 — Criar Conta Bancária

> Como utilizador, quero adicionar as minhas contas bancárias, para centralizar a minha informação financeira.

### Cenário 4.1: Criar conta com sucesso
```
Dado que estou autenticado
Quando clico em "Nova Conta"
E preencho nome "Caixa Geral", tipo "Conta Corrente", saldo "1500"
E clico em "Criar Conta"
Então a conta é criada com sucesso
E aparece na minha lista de contas com saldo 1500.00 EUR
```

### Cenário 4.2: Nome do banco vazio
```
Dado que estou no modal de criar conta
Quando deixo o campo "Nome do banco" vazio
E clico em "Criar Conta"
Então vejo a mensagem "Nome do banco é obrigatório"
E a conta não é criada
```

---

## US05 — Adicionar Transação Manual

> Como utilizador, quero registar transações manualmente, para manter um histórico financeiro completo.

### Cenário 5.1: Adicionar despesa com categoria
```
Dado que tenho a conta "Caixa Geral" registada
Quando clico em "Nova Transação"
E preencho descrição "Supermercado Continente", valor "-45.30", data "2026-01-15"
E seleciono a categoria "Supermercado"
E clico em "Adicionar"
Então a transação é guardada com categoria "Supermercado"
E aparece na lista de transações
```

### Cenário 5.2: Adicionar transação sem categoria (categorização automática)
```
Dado que tenho a conta "Caixa Geral" registada
Quando adiciono transação com descrição "Uber Eats" sem selecionar categoria
Então o sistema categoriza automaticamente como "Restauração"
E a transação é guardada com confiança de categorização > 0
```

### Cenário 5.3: Adicionar receita
```
Dado que tenho a conta "Caixa Geral" registada
Quando adiciono transação com descrição "Salário" e valor "1500.00"
Então a transação é guardada como receita (valor positivo)
E aparece na lista com ícone de receita
```

---

## US06 — Importar Transações via CSV

> Como utilizador, quero importar um extrato bancário em CSV, para evitar registo manual de muitas transações.

### Cenário 6.1: Importação com sucesso
```
Dado que tenho a conta "Caixa Geral" registada
Quando clico em "Importar CSV"
E seleciono a conta de destino "Caixa Geral"
E carrego um ficheiro CSV com 20 linhas válidas
Então vejo pré-visualização das primeiras 5 linhas
E o sistema indica "20 linhas detetadas"
Quando clico em "Confirmar Import"
Então 20 transações são importadas com sucesso
E cada transação é categorizada automaticamente
```

### Cenário 6.2: Ficheiro inválido
```
Dado que estou no modal de importação CSV
Quando carrego um ficheiro com extensão .xlsx
Então vejo a mensagem "Apenas ficheiros CSV são aceites"
E o botão "Confirmar" permanece desativado
```

### Cenário 6.3: CSV com formato incorreto
```
Dado que carrego um CSV sem a coluna "date"
Quando clico em "Confirmar Import"
Então vejo a mensagem "Erro ao importar. Verifica o formato do ficheiro"
E nenhuma transação é importada
```

---

## US07 — Listar e Filtrar Transações

> Como utilizador, quero ver as minhas transações organizadas por data, para acompanhar os meus movimentos financeiros.

### Cenário 7.1: Listar transações recentes
```
Dado que tenho 30 transações registadas
Quando acedo à página de transações
Então vejo as transações ordenadas por data (mais recente primeiro)
E cada transação mostra descrição, valor, categoria e data
E as transações estão agrupadas por dia
```

### Cenário 7.2: Filtrar por tipo
```
Dado que tenho transações de receita e despesa
Quando seleciono o filtro "Despesas"
Então apenas transações com valor negativo são apresentadas
```

### Cenário 7.3: Sem transações
```
Dado que sou um utilizador novo sem transações
Quando acedo à página de transações
Então vejo mensagem "Sem transações" com botão para adicionar
```

---

## US08 — Criar Orçamento

> Como utilizador, quero definir limites de gastos por categoria, para controlar melhor as minhas despesas.

### Cenário 8.1: Criar orçamento com sucesso
```
Dado que estou na página de orçamentos
Quando clico em "Novo Orçamento"
E seleciono categoria "Restauração" com limite "200" EUR
E clico em "Criar"
Então o orçamento é criado para o período atual
E vejo um card com barra de progresso a 0%
```

### Cenário 8.2: Ver progresso do orçamento
```
Dado que tenho orçamento de 200 EUR para "Restauração"
E já gastei 120 EUR nessa categoria este mês
Quando acedo à página de orçamentos
Então vejo a barra de progresso a 60%
E o valor "120 / 200 EUR" é apresentado
```

---

## US09 — Ver Dashboard

> Como utilizador, quero ver um painel geral da minha saúde financeira, para ter uma visão rápida do meu estado atual.

### Cenário 9.1: Dashboard com dados
```
Dado que tenho 2 contas com saldo total de 3500 EUR
E tenho transações no mês corrente
Quando acedo ao dashboard
Então vejo o saldo total "3 500,00 EUR"
E vejo o resumo de receitas e despesas do mês
E vejo as 5 transações mais recentes
E vejo o gráfico de despesas por categoria
```

### Cenário 9.2: Primeiro acesso (onboarding)
```
Dado que sou um utilizador novo sem contas
Quando acedo ao dashboard pela primeira vez
Então o wizard de onboarding é apresentado
E guia-me em 4 passos: boas-vindas, criar conta, primeira transação, definir orçamento
E posso saltar qualquer passo
```

---

## US10 — Editar Perfil

> Como utilizador, quero editar os meus dados pessoais, para manter o meu perfil atualizado.

### Cenário 10.1: Atualizar nome e rendimento
```
Dado que estou na página de perfil
Quando altero o nome para "João M. Silva"
E altero o rendimento mensal para "1800"
E clico em "Guardar Alterações"
Então os dados são atualizados com sucesso
E vejo uma notificação de confirmação
```

### Cenário 10.2: Ver contas bancárias no perfil
```
Dado que tenho 3 contas bancárias registadas
Quando acedo à página de perfil
Então vejo a secção "Contas Bancárias" com as 3 contas
E cada conta mostra nome do banco, tipo e saldo
```

---

## Resumo de Cobertura

| User Story | Cenários | Requisitos Cobertos |
|-----------|:--------:|:-------------------:|
| US01 Registo | 3 | RF01 |
| US02 Login | 3 | RF02 |
| US03 Logout | 1 | RF02 |
| US04 Criar Conta | 2 | RF03 |
| US05 Adicionar Transação | 3 | RF04, RF08 |
| US06 Importar CSV | 3 | RF05, RF08 |
| US07 Listar Transações | 3 | RF06 |
| US08 Criar Orçamento | 2 | RF09 |
| US09 Ver Dashboard | 2 | RF07 |
| US10 Editar Perfil | 2 | RF10 |
| **Total** | **24** | **RF01–RF10** |
