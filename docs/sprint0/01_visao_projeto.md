# FinTwin — Visão do Projeto

## Informações Gerais

| Campo | Valor |
|-------|-------|
| **Projeto** | FinTwin — Gémeo Digital Financeiro |
| **Aluno** | Henrique Laia |
| **Curso** | Licenciatura em Engenharia Informática |
| **Universidade** | Universidade da Beira Interior (UBI) |
| **Disciplina** | Engenharia de Software 2025/26 |
| **Data** | Março 2026 |

---

## 1. Problema

A gestão financeira pessoal continua a ser um desafio para a maioria das pessoas. Os principais problemas identificados são:

- **Fragmentação**: Contas bancárias, despesas e receitas estão dispersas por múltiplas plataformas e extratos
- **Falta de visibilidade**: Dificuldade em ter uma visão global do estado financeiro atual
- **Ausência de controlo orçamental**: Poucos utilizadores definem ou acompanham orçamentos mensais por categoria
- **Categorização manual**: Classificar transações é um processo tedioso que poucos mantêm atualizado
- **Falta de contexto**: Dados financeiros sem análise não geram ação

---

## 2. Solução Proposta

O **FinTwin** é uma aplicação web de gestão financeira pessoal que funciona como um "gémeo digital financeiro" do utilizador. A aplicação centraliza:

- **Contas bancárias** — registo manual de múltiplas contas (corrente, poupança, investimento)
- **Transações** — registo manual, importação via CSV, e categorização automática
- **Orçamentos** — definição de limites mensais por categoria com acompanhamento visual
- **Dashboard** — painel central com saldo total, resumo de receitas/despesas, e transações recentes

---

## 3. Objetivos

1. Centralizar a informação financeira do utilizador numa única plataforma
2. Permitir o registo e categorização eficiente de transações financeiras
3. Oferecer um dashboard visual com indicadores-chave do estado financeiro
4. Suportar a definição e monitorização de orçamentos mensais por categoria
5. Garantir segurança dos dados com autenticação JWT e encriptação de passwords

---

## 4. Âmbito — Sprint 1 vs Roadmap Futuro

| Funcionalidade | Sprint 1 (MVP) | Sprints Futuros |
|----------------|:--------------:|:---------------:|
| Registo e autenticação (JWT) | X | |
| CRUD de contas bancárias | X | |
| CRUD de transações manuais | X | |
| Importação CSV | X | |
| Categorização automática | X | |
| Dashboard com saldo e resumo | X | |
| Orçamentos por categoria | X | |
| Perfil do utilizador | X | |
| Dark mode e responsive design | X | |
| Score financeiro (gamificação) | | X |
| Simulações Monte Carlo | | X |
| Desafios de poupança | | X |
| Gémeo digital (projeções IA) | | X |
| Integração Open Banking (Plaid) | | X |
| Relatórios PDF partilháveis | | X |
| Notificações em tempo real (SSE) | | X |

---

## 5. Público-Alvo

- Jovens adultos (18-35 anos) que querem organizar as suas finanças pessoais
- Utilizadores que usam múltiplas contas bancárias e querem uma visão consolidada
- Pessoas que querem começar a definir e acompanhar orçamentos mensais

---

## 6. Diferenciadores

- **Categorização inteligente** — a aplicação sugere automaticamente categorias para as transações com base na descrição
- **Importação CSV** — permite importar extratos bancários em formato CSV, evitando entrada manual
- **Interface moderna** — design system premium com suporte a dark mode
- **Arquitetura escalável** — backend async com FastAPI, preparado para futuras integrações (Open Banking, IA)
