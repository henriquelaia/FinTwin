# FinTwin — Visao do Projeto

## Informacoes Gerais

| Campo | Valor |
|-------|-------|
| **Projeto** | FinTwin — Gemeo Digital Financeiro |
| **Aluno** | Henrique |
| **Curso** | Licenciatura em Engenharia Informatica |
| **Universidade** | Universidade da Beira Interior (UBI) |
| **Disciplina** | Engenharia de Software 2025/26 |
| **Data** | Marco 2026 |

---

## 1. Problema

A gestao financeira pessoal continua a ser um desafio para a maioria das pessoas. Os principais problemas identificados sao:

- **Fragmentacao**: Contas bancarias, despesas e receitas estao dispersas por multiplas plataformas e extratos
- **Falta de visibilidade**: Dificuldade em ter uma visao global do estado financeiro atual
- **Ausencia de controlo orcamental**: Poucos utilizadores definem ou acompanham orcamentos mensais por categoria
- **Categorizacao manual**: Classificar transacoes e um processo tedioso que poucos manteem atualizado
- **Falta de contexto**: Dados financeiros sem analise nao geram acao

---

## 2. Solucao Proposta

O **FinTwin** e uma aplicacao web de gestao financeira pessoal que funciona como um "gemeo digital financeiro" do utilizador. A aplicacao centraliza:

- **Contas bancarias** — registo manual de multiplas contas (corrente, poupanca, investimento)
- **Transacoes** — registo manual, importacao via CSV, e categorizacao automatica
- **Orcamentos** — definicao de limites mensais por categoria com acompanhamento visual
- **Dashboard** — painel central com saldo total, resumo de receitas/despesas, e transacoes recentes

---

## 3. Objetivos

1. Centralizar a informacao financeira do utilizador numa unica plataforma
2. Permitir o registo e categorizacao eficiente de transacoes financeiras
3. Oferecer um dashboard visual com indicadores-chave do estado financeiro
4. Suportar a definicao e monitorizacao de orcamentos mensais por categoria
5. Garantir seguranca dos dados com autenticacao JWT e encriptacao de passwords

---

## 4. Ambito — Sprint 1 vs Roadmap Futuro

| Funcionalidade | Sprint 1 (MVP) | Sprints Futuros |
|----------------|:--------------:|:---------------:|
| Registo e autenticacao (JWT) | X | |
| CRUD de contas bancarias | X | |
| CRUD de transacoes manuais | X | |
| Importacao CSV | X | |
| Categorizacao automatica | X | |
| Dashboard com saldo e resumo | X | |
| Orcamentos por categoria | X | |
| Perfil do utilizador | X | |
| Dark mode e responsive design | X | |
| Score financeiro (gamificacao) | | X |
| Simulacoes Monte Carlo | | X |
| Desafios de poupanca | | X |
| Gemeo digital (projecoes IA) | | X |
| Integracao Open Banking (Plaid) | | X |
| Relatorios PDF partilhaveis | | X |
| Notificacoes em tempo real (SSE) | | X |

---

## 5. Publico-Alvo

- Jovens adultos (18-35 anos) que querem organizar as suas financas pessoais
- Utilizadores que usam multiplas contas bancarias e querem uma visao consolidada
- Pessoas que querem comecar a definir e acompanhar orcamentos mensais

---

## 6. Diferenciadores

- **Categorizacao inteligente** — a aplicacao sugere automaticamente categorias para as transacoes com base na descricao
- **Importacao CSV** — permite importar extratos bancarios em formato CSV, evitando entrada manual
- **Interface moderna** — design system premium com suporte a dark mode
- **Arquitetura escalavel** — backend async com FastAPI, preparado para futuras integracoes (Open Banking, IA)
