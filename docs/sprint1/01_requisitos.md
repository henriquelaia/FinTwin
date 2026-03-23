# FinTwin — Requisitos (Sprint 1)

## 1. Requisitos Funcionais

### Autenticação

| ID | Requisito | Prioridade | Caso de Uso |
|----|-----------|:----------:|:-----------:|
| RF01 | O sistema deve permitir o registo de novos utilizadores com nome, email e password | Alta | UC01 |
| RF02 | O sistema deve permitir login/logout com autenticação JWT | Alta | UC02, UC03 |

### Contas Bancárias

| ID | Requisito | Prioridade | Caso de Uso |
|----|-----------|:----------:|:-----------:|
| RF03 | O sistema deve permitir criar, listar, editar e eliminar contas bancárias (nome, tipo, saldo) | Alta | UC04–UC07 |

### Transações

| ID | Requisito | Prioridade | Caso de Uso |
|----|-----------|:----------:|:-----------:|
| RF04 | O sistema deve permitir adicionar e editar transações manuais (descrição, valor, data, categoria) | Alta | UC08, UC11 |
| RF05 | O sistema deve permitir importar transações a partir de ficheiros CSV | Alta | UC10 |
| RF06 | O sistema deve listar transações com paginação, agrupamento por data e filtros (tipo, categoria) | Alta | UC09 |

### Dashboard e Visualização

| ID | Requisito | Prioridade | Caso de Uso |
|----|-----------|:----------:|:-----------:|
| RF07 | O sistema deve apresentar um dashboard com saldo total, resumo mensal (receitas/despesas) e transações recentes | Alta | UC15 |

### Categorização

| ID | Requisito | Prioridade | Caso de Uso |
|----|-----------|:----------:|:-----------:|
| RF08 | O sistema deve categorizar transações automaticamente com base na descrição, usando categorias predefinidas | Média | UC08, UC10 |

### Orçamentos

| ID | Requisito | Prioridade | Caso de Uso |
|----|-----------|:----------:|:-----------:|
| RF09 | O sistema deve permitir criar, visualizar e eliminar orçamentos mensais por categoria, com barra de progresso | Alta | UC12–UC14 |

### Perfil

| ID | Requisito | Prioridade | Caso de Uso |
|----|-----------|:----------:|:-----------:|
| RF10 | O sistema deve permitir ao utilizador editar os seus dados pessoais (nome, rendimento, moeda) | Média | UC16 |

---

## 2. Requisitos Não-Funcionais

### Performance

| ID | Requisito | Métrica |
|----|-----------|---------|
| RNF01 | O tempo de resposta da API deve ser inferior a 500ms para 95% dos requests | p95 < 500ms |

### Segurança

| ID | Requisito | Implementação |
|----|-----------|---------------|
| RNF02 | As passwords devem ser armazenadas com hashing seguro | bcrypt com 12 rounds |
| RNF03 | Os endpoints de autenticação devem ter rate limiting | Login: 10/min, Registo: 5/min (SlowAPI) |

### Usabilidade

| ID | Requisito | Implementação |
|----|-----------|---------------|
| RNF04 | A interface deve ser responsiva e funcional em dispositivos móveis (>= 375px) | Mobile-first com Tailwind CSS |
| RNF05 | A aplicação deve suportar modo escuro (dark mode) | CSS variables + ThemeProvider + localStorage |

### Infraestrutura

| ID | Requisito | Implementação |
|----|-----------|---------------|
| RNF06 | A aplicação deve ser containerizada para facilitar deploy e desenvolvimento | Docker Compose (4 containers) |

---

## 3. Regras de Negócio

| ID | Regra |
|----|-------|
| RN01 | Transações com valor negativo são despesas; valor positivo são receitas |
| RN02 | Cada transação pertence a exatamente uma conta bancária |
| RN03 | Uma transação pode ter no máximo uma categoria (ou nenhuma) |
| RN04 | Ao eliminar uma conta bancária, todas as suas transações são eliminadas (CASCADE) |
| RN05 | Ao eliminar uma categoria, as transações associadas mantêm-se sem categoria (SET NULL) |
| RN06 | Emails de utilizadores devem ser únicos no sistema |
| RN07 | O saldo total no dashboard é a soma dos saldos de todas as contas do utilizador |
| RN08 | A categorização automática usa a descrição e o nome do comerciante para inferir a categoria |
| RN09 | Orçamentos são definidos por categoria e período; a barra de progresso mostra % do limite gasto |

---

## 4. Restrições Técnicas

| Restrição | Detalhe |
|-----------|---------|
| Base de dados | PostgreSQL 15 (relacional, suporte a UUIDs nativos) |
| Backend | Python 3.12, FastAPI (async) |
| Frontend | React 18, TypeScript strict |
| Comunicação | REST API com JSON, versionada (/api/v1) |
| Autenticação | JWT Bearer token (stateless) |
| Containerização | Docker Compose obrigatório para desenvolvimento |

---

## 5. Critérios de Aceitação Globais

- [ ] Todos os endpoints da API retornam respostas JSON com códigos HTTP corretos
- [ ] Login com credenciais erradas retorna 401, nunca revela se o email existe
- [ ] Rate limiting ativo nos endpoints de auth (verificável com testes)
- [ ] Interface funcional em Chrome, Firefox e Safari (últimas 2 versões)
- [ ] Dark mode funciona em todas as páginas sem quebras visuais
- [ ] Nenhuma credencial ou segredo hardcoded no código fonte
