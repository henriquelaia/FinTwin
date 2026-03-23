# FinTwin — Cenarios e User Stories (Sprint 1)

## Formato

Cada user story segue o formato:
- **Como** [ator], **quero** [acao], **para** [beneficio]
- Cenarios no formato Gherkin: **Dado** / **Quando** / **Entao**

---

## US01 — Registo de Utilizador

> Como visitante, quero criar uma conta no FinTwin, para comecar a gerir as minhas financas.

### Cenario 1.1: Registo com sucesso
```
Dado que nao tenho conta no sistema
Quando preencho nome "Joao Silva", email "joao@email.com" e password "secret123"
E clico em "Criar Conta"
Entao a conta e criada com sucesso
E recebo um token JWT
E sou redirecionado para o wizard de onboarding
```

### Cenario 1.2: Email ja registado
```
Dado que ja existe uma conta com email "joao@email.com"
Quando tento registar com o mesmo email
Entao vejo a mensagem "Este email ja esta registado"
E permaneco na pagina de registo
```

### Cenario 1.3: Password demasiado curta
```
Dado que estou na pagina de registo
Quando preencho uma password com menos de 6 caracteres
E clico em "Criar Conta"
Entao vejo a mensagem de validacao "Password demasiado curta"
E a conta nao e criada
```

---

## US02 — Login

> Como utilizador registado, quero fazer login, para aceder ao meu dashboard financeiro.

### Cenario 2.1: Login com sucesso
```
Dado que tenho conta com email "joao@email.com" e password "secret123"
Quando preencho as credenciais corretas
E clico em "Entrar"
Entao recebo um token JWT
E sou redirecionado para o dashboard
```

### Cenario 2.2: Credenciais incorretas
```
Dado que tenho conta com email "joao@email.com"
Quando preencho password incorreta
E clico em "Entrar"
Entao vejo a mensagem "Email ou password incorretos"
E permaneco na pagina de login
```

### Cenario 2.3: Rate limit excedido
```
Dado que fiz 10 tentativas de login no ultimo minuto
Quando tento fazer login novamente
Entao recebo resposta HTTP 429
E vejo mensagem "Demasiadas tentativas, tenta novamente mais tarde"
```

---

## US03 — Logout

> Como utilizador autenticado, quero fazer logout, para terminar a minha sessao de forma segura.

### Cenario 3.1: Logout com sucesso
```
Dado que estou autenticado no sistema
Quando clico no botao "Sair" na sidebar
Entao o token JWT e removido do localStorage
E sou redirecionado para a pagina de login
```

---

## US04 — Criar Conta Bancaria

> Como utilizador, quero adicionar as minhas contas bancarias, para centralizar a minha informacao financeira.

### Cenario 4.1: Criar conta com sucesso
```
Dado que estou autenticado
Quando clico em "Nova Conta"
E preencho nome "Caixa Geral", tipo "Conta Corrente", saldo "1500"
E clico em "Criar Conta"
Entao a conta e criada com sucesso
E aparece na minha lista de contas com saldo 1500.00 EUR
```

### Cenario 4.2: Nome do banco vazio
```
Dado que estou no modal de criar conta
Quando deixo o campo "Nome do banco" vazio
E clico em "Criar Conta"
Entao vejo a mensagem "Nome do banco e obrigatorio"
E a conta nao e criada
```

---

## US05 — Adicionar Transacao Manual

> Como utilizador, quero registar transacoes manualmente, para manter um historico financeiro completo.

### Cenario 5.1: Adicionar despesa com categoria
```
Dado que tenho a conta "Caixa Geral" registada
Quando clico em "Nova Transacao"
E preencho descricao "Supermercado Continente", valor "-45.30", data "2026-01-15"
E seleciono a categoria "Supermercado"
E clico em "Adicionar"
Entao a transacao e guardada com categoria "Supermercado"
E aparece na lista de transacoes
```

### Cenario 5.2: Adicionar transacao sem categoria (categorizacao automatica)
```
Dado que tenho a conta "Caixa Geral" registada
Quando adiciono transacao com descricao "Uber Eats" sem selecionar categoria
Entao o sistema categoriza automaticamente como "Restauracao"
E a transacao e guardada com confianca de categorizacao > 0
```

### Cenario 5.3: Adicionar receita
```
Dado que tenho a conta "Caixa Geral" registada
Quando adiciono transacao com descricao "Salario" e valor "1500.00"
Entao a transacao e guardada como receita (valor positivo)
E aparece na lista com icone de receita
```

---

## US06 — Importar Transacoes via CSV

> Como utilizador, quero importar um extrato bancario em CSV, para evitar registo manual de muitas transacoes.

### Cenario 6.1: Importacao com sucesso
```
Dado que tenho a conta "Caixa Geral" registada
Quando clico em "Importar CSV"
E seleciono a conta de destino "Caixa Geral"
E carrego um ficheiro CSV com 20 linhas validas
Entao vejo pre-visualizacao das primeiras 5 linhas
E o sistema indica "20 linhas detetadas"
Quando clico em "Confirmar Import"
Entao 20 transacoes sao importadas com sucesso
E cada transacao e categorizada automaticamente
```

### Cenario 6.2: Ficheiro invalido
```
Dado que estou no modal de importacao CSV
Quando carrego um ficheiro com extensao .xlsx
Entao vejo a mensagem "Apenas ficheiros CSV sao aceites"
E o botao "Confirmar" permanece desativado
```

### Cenario 6.3: CSV com formato incorreto
```
Dado que carrego um CSV sem a coluna "date"
Quando clico em "Confirmar Import"
Entao vejo a mensagem "Erro ao importar. Verifica o formato do ficheiro"
E nenhuma transacao e importada
```

---

## US07 — Listar e Filtrar Transacoes

> Como utilizador, quero ver as minhas transacoes organizadas por data, para acompanhar os meus movimentos financeiros.

### Cenario 7.1: Listar transacoes recentes
```
Dado que tenho 30 transacoes registadas
Quando acedo a pagina de transacoes
Entao vejo as transacoes ordenadas por data (mais recente primeiro)
E cada transacao mostra descricao, valor, categoria e data
E as transacoes estao agrupadas por dia
```

### Cenario 7.2: Filtrar por tipo
```
Dado que tenho transacoes de receita e despesa
Quando seleciono o filtro "Despesas"
Entao apenas transacoes com valor negativo sao apresentadas
```

### Cenario 7.3: Sem transacoes
```
Dado que sou um utilizador novo sem transacoes
Quando acedo a pagina de transacoes
Entao vejo mensagem "Sem transacoes" com botao para adicionar
```

---

## US08 — Criar Orcamento

> Como utilizador, quero definir limites de gastos por categoria, para controlar melhor as minhas despesas.

### Cenario 8.1: Criar orcamento com sucesso
```
Dado que estou na pagina de orcamentos
Quando clico em "Novo Orcamento"
E seleciono categoria "Restauracao" com limite "200" EUR
E clico em "Criar"
Entao o orcamento e criado para o periodo atual
E vejo um card com barra de progresso a 0%
```

### Cenario 8.2: Ver progresso do orcamento
```
Dado que tenho orcamento de 200 EUR para "Restauracao"
E ja gastei 120 EUR nessa categoria este mes
Quando acedo a pagina de orcamentos
Entao vejo a barra de progresso a 60%
E o valor "120 / 200 EUR" e apresentado
```

---

## US09 — Ver Dashboard

> Como utilizador, quero ver um painel geral da minha saude financeira, para ter uma visao rapida do meu estado atual.

### Cenario 9.1: Dashboard com dados
```
Dado que tenho 2 contas com saldo total de 3500 EUR
E tenho transacoes no mes corrente
Quando acedo ao dashboard
Entao vejo o saldo total "3 500,00 EUR"
E vejo o resumo de receitas e despesas do mes
E vejo as 5 transacoes mais recentes
E vejo o grafico de despesas por categoria
```

### Cenario 9.2: Primeiro acesso (onboarding)
```
Dado que sou um utilizador novo sem contas
Quando acedo ao dashboard pela primeira vez
Entao o wizard de onboarding e apresentado
E guia-me em 4 passos: boas-vindas, criar conta, primeira transacao, definir orcamento
E posso saltar qualquer passo
```

---

## US10 — Editar Perfil

> Como utilizador, quero editar os meus dados pessoais, para manter o meu perfil atualizado.

### Cenario 10.1: Atualizar nome e rendimento
```
Dado que estou na pagina de perfil
Quando altero o nome para "Joao M. Silva"
E altero o rendimento mensal para "1800"
E clico em "Guardar Alteracoes"
Entao os dados sao atualizados com sucesso
E vejo uma notificacao de confirmacao
```

### Cenario 10.2: Ver contas bancarias no perfil
```
Dado que tenho 3 contas bancarias registadas
Quando acedo a pagina de perfil
Entao vejo a seccao "Contas Bancarias" com as 3 contas
E cada conta mostra nome do banco, tipo e saldo
```

---

## Resumo de Cobertura

| User Story | Cenarios | Requisitos Cobertos |
|-----------|:--------:|:-------------------:|
| US01 Registo | 3 | RF01 |
| US02 Login | 3 | RF02 |
| US03 Logout | 1 | RF02 |
| US04 Criar Conta | 2 | RF03 |
| US05 Adicionar Transacao | 3 | RF04, RF08 |
| US06 Importar CSV | 3 | RF05, RF08 |
| US07 Listar Transacoes | 3 | RF06 |
| US08 Criar Orcamento | 2 | RF09 |
| US09 Ver Dashboard | 2 | RF07 |
| US10 Editar Perfil | 2 | RF10 |
| **Total** | **24** | **RF01–RF10** |
