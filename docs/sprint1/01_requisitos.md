# FinTwin — Requisitos (Sprint 1)

## 1. Requisitos Funcionais

### Autenticacao

| ID | Requisito | Prioridade | Caso de Uso |
|----|-----------|:----------:|:-----------:|
| RF01 | O sistema deve permitir o registo de novos utilizadores com nome, email e password | Alta | UC01 |
| RF02 | O sistema deve permitir login/logout com autenticacao JWT | Alta | UC02, UC03 |

### Contas Bancarias

| ID | Requisito | Prioridade | Caso de Uso |
|----|-----------|:----------:|:-----------:|
| RF03 | O sistema deve permitir criar, listar, editar e eliminar contas bancarias (nome, tipo, saldo) | Alta | UC04–UC07 |

### Transacoes

| ID | Requisito | Prioridade | Caso de Uso |
|----|-----------|:----------:|:-----------:|
| RF04 | O sistema deve permitir adicionar e editar transacoes manuais (descricao, valor, data, categoria) | Alta | UC08, UC11 |
| RF05 | O sistema deve permitir importar transacoes a partir de ficheiros CSV | Alta | UC10 |
| RF06 | O sistema deve listar transacoes com paginacao, agrupamento por data e filtros (tipo, categoria) | Alta | UC09 |

### Dashboard e Visualizacao

| ID | Requisito | Prioridade | Caso de Uso |
|----|-----------|:----------:|:-----------:|
| RF07 | O sistema deve apresentar um dashboard com saldo total, resumo mensal (receitas/despesas) e transacoes recentes | Alta | UC15 |

### Categorizacao

| ID | Requisito | Prioridade | Caso de Uso |
|----|-----------|:----------:|:-----------:|
| RF08 | O sistema deve categorizar transacoes automaticamente com base na descricao, usando categorias predefinidas | Media | UC08, UC10 |

### Orcamentos

| ID | Requisito | Prioridade | Caso de Uso |
|----|-----------|:----------:|:-----------:|
| RF09 | O sistema deve permitir criar, visualizar e eliminar orcamentos mensais por categoria, com barra de progresso | Alta | UC12–UC14 |

### Perfil

| ID | Requisito | Prioridade | Caso de Uso |
|----|-----------|:----------:|:-----------:|
| RF10 | O sistema deve permitir ao utilizador editar os seus dados pessoais (nome, rendimento, moeda) | Media | UC16 |

---

## 2. Requisitos Nao-Funcionais

### Performance

| ID | Requisito | Metrica |
|----|-----------|---------|
| RNF01 | O tempo de resposta da API deve ser inferior a 500ms para 95% dos requests | p95 < 500ms |

### Seguranca

| ID | Requisito | Implementacao |
|----|-----------|---------------|
| RNF02 | As passwords devem ser armazenadas com hashing seguro | bcrypt com 12 rounds |
| RNF03 | Os endpoints de autenticacao devem ter rate limiting | Login: 10/min, Registo: 5/min (SlowAPI) |

### Usabilidade

| ID | Requisito | Implementacao |
|----|-----------|---------------|
| RNF04 | A interface deve ser responsiva e funcional em dispositivos moveis (>= 375px) | Mobile-first com Tailwind CSS |
| RNF05 | A aplicacao deve suportar modo escuro (dark mode) | CSS variables + ThemeProvider + localStorage |

### Infraestrutura

| ID | Requisito | Implementacao |
|----|-----------|---------------|
| RNF06 | A aplicacao deve ser containerizada para facilitar deploy e desenvolvimento | Docker Compose (4 containers) |

---

## 3. Regras de Negocio

| ID | Regra |
|----|-------|
| RN01 | Transacoes com valor negativo sao despesas; valor positivo sao receitas |
| RN02 | Cada transacao pertence a exatamente uma conta bancaria |
| RN03 | Uma transacao pode ter no maximo uma categoria (ou nenhuma) |
| RN04 | Ao eliminar uma conta bancaria, todas as suas transacoes sao eliminadas (CASCADE) |
| RN05 | Ao eliminar uma categoria, as transacoes associadas mantem-se sem categoria (SET NULL) |
| RN06 | Emails de utilizadores devem ser unicos no sistema |
| RN07 | O saldo total no dashboard e a soma dos saldos de todas as contas do utilizador |
| RN08 | A categorizacao automatica usa a descricao e o nome do comerciante para inferir a categoria |
| RN09 | Orcamentos sao definidos por categoria e periodo; a barra de progresso mostra % do limite gasto |

---

## 4. Restricoes Tecnicas

| Restricao | Detalhe |
|-----------|---------|
| Base de dados | PostgreSQL 15 (relacional, suporte a UUIDs nativos) |
| Backend | Python 3.12, FastAPI (async) |
| Frontend | React 18, TypeScript strict |
| Comunicacao | REST API com JSON, versionada (/api/v1) |
| Autenticacao | JWT Bearer token (stateless) |
| Containerizacao | Docker Compose obrigatorio para desenvolvimento |

---

## 5. Criterios de Aceitacao Globais

- [ ] Todos os endpoints da API retornam respostas JSON com codigos HTTP corretos
- [ ] Login com credenciais erradas retorna 401, nunca revela se o email existe
- [ ] Rate limiting ativo nos endpoints de auth (verificavel com testes)
- [ ] Interface funcional em Chrome, Firefox e Safari (ultimas 2 versoes)
- [ ] Dark mode funciona em todas as paginas sem quebras visuais
- [ ] Nenhuma credencial ou segredo hardcoded no codigo fonte
