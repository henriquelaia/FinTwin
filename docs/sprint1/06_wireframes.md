# FinTwin — Wireframes (Sprint 1)

## Convenções

```
┌─────────┐  Caixa = container/card
│         │
└─────────┘

[  Botão  ]  Botão clicável

( campo )    Input field

── ── ── ──  Separador/divider

▼            Dropdown/select

●            Ícone/avatar
```

---

## W01 — Página de Login

```
┌──────────────────────────────────────────────────┐
│                                                  │
│              ┌──────────────────────┐            │
│              │                      │            │
│              │     ● FinTwin Logo   │            │
│              │                      │            │
│              │   ( Email          ) │            │
│              │                      │            │
│              │   ( Password       ) │            │
│              │                      │            │
│              │   [ Entrar         ] │            │
│              │                      │            │
│              │   Não tens conta?    │            │
│              │   Criar conta        │            │
│              │                      │            │
│              └──────────────────────┘            │
│                                                  │
│           Background: gradiente teal             │
└──────────────────────────────────────────────────┘
```

**Elementos:**
- Logo FinTwin centrado
- Campos de email e password (input-field)
- Botão primário "Entrar" (btn-primary, full-width)
- Link para página de registo
- Background com gradiente da cor brand

**Interações:**
- Enter submete o formulário
- Erro de login mostra mensagem abaixo do formulário
- Após login, redireciona para Dashboard

---

## W02 — Página de Registo

```
┌──────────────────────────────────────────────────┐
│                                                  │
│              ┌──────────────────────┐            │
│              │                      │            │
│              │     ● FinTwin Logo   │            │
│              │                      │            │
│              │   ( Nome Completo  ) │            │
│              │   ( Email          ) │            │
│              │   ( Password       ) │            │
│              │   ( Rendimento   ) ▼ Moeda │      │
│              │                      │            │
│              │   [ Criar Conta    ] │            │
│              │                      │            │
│              │   Já tens conta?     │            │
│              │   Entrar             │            │
│              │                      │            │
│              └──────────────────────┘            │
│                                                  │
└──────────────────────────────────────────────────┘
```

**Elementos:**
- Campos: nome, email, password (obrigatórios), rendimento e moeda (opcionais)
- Dropdown de moeda (EUR, USD, GBP)
- Botão "Criar Conta"
- Link para login

---

## W03 — Dashboard

```
┌─────────────────────────────────────────────────────────────────┐
│ ● Sidebar        │  Header: glass panel + search + dark toggle │
│                   │─────────────────────────────────────────────│
│ ▸ Dashboard      │                                              │
│   Transações     │  BOM DIA, JOÃO                               │
│   Orçamentos     │                                              │
│   Perfil         │  ┌──────────────┐ ┌──────────┐ ┌──────────┐ │
│                   │  │ SALDO TOTAL  │ │ RECEITAS │ │ DESPESAS │ │
│                   │  │              │ │          │ │          │ │
│                   │  │  3 500,00 €  │ │ 1 500 € │ │  -820 € │ │
│                   │  │              │ │  ↑ 12%   │ │  ↓ 5%   │ │
│                   │  └──────────────┘ └──────────┘ └──────────┘ │
│   ── ── ── ──    │                                              │
│                   │  ÚLTIMAS TRANSAÇÕES          [Ver todas →]  │
│   Sair           │  ┌───────────────────────────────────────┐  │
│                   │  │ ● Continente    -45.30€   15 Jan     │  │
│                   │  │ ● Salário      +1500.00€  01 Jan     │  │
│                   │  │ ● Uber Eats     -12.50€   14 Jan     │  │
│                   │  │ ● Renda        -600.00€   01 Jan     │  │
│                   │  │ ● Spotify       -10.99€   10 Jan     │  │
│                   │  └───────────────────────────────────────┘  │
│                   │                                              │
│                   │  DESPESAS POR CATEGORIA                      │
│                   │  ┌───────────────────────┐                  │
│                   │  │    ╭──╮                │                  │
│                   │  │   ╱    ╲   Pie Chart   │                  │
│                   │  │  │      │  por categoria│                  │
│                   │  │   ╲    ╱               │                  │
│                   │  │    ╰──╯                │                  │
│                   │  └───────────────────────┘                  │
└───────────────────┴─────────────────────────────────────────────┘
```

**Elementos:**
- Sidebar: navegação principal, toggle dark mode, botão sair
- Greeting como micro-label ("BOM DIA, JOÃO")
- 3 stat cards: saldo total (hero number), receitas, despesas
- Lista das 5 transações mais recentes (TransactionRow)
- Gráfico pie chart de despesas por categoria
- Se utilizador novo: wizard de onboarding sobreposto

---

## W04 — Página de Transações

```
┌─────────────────────────────────────────────────────────────────┐
│ Sidebar          │  TRANSAÇÕES                                  │
│                   │                                              │
│                   │  ┌──────────┐ ┌──────────┐ ┌──────────┐    │
│                   │  │ TOTAL    │ │ RECEITAS │ │ DESPESAS │    │
│                   │  │ 45 txns  │ │ 1 500 €  │ │  -820 €  │    │
│                   │  └──────────┘ └──────────┘ └──────────┘    │
│                   │                                              │
│                   │  [+ Nova Transação]  [Importar CSV]          │
│                   │                                              │
│                   │  Filtros: (Todas) (Receitas) (Despesas)      │
│                   │           ▼ Categoria   ▼ Período            │
│                   │                                              │
│                   │  HOJE — 22 MAR 2026                          │
│                   │  ┌───────────────────────────────────────┐  │
│                   │  │ ● Supermercado    -45.30€  Continente │  │
│                   │  └───────────────────────────────────────┘  │
│                   │  ┌───────────────────────────────────────┐  │
│                   │  │ ● Almoço          -8.50€   Restaurante│  │
│                   │  └───────────────────────────────────────┘  │
│                   │                                              │
│                   │  ONTEM — 21 MAR 2026                         │
│                   │  ┌───────────────────────────────────────┐  │
│                   │  │ ● Uber            -6.20€   Transportes│  │
│                   │  └───────────────────────────────────────┘  │
│                   │  ┌───────────────────────────────────────┐  │
│                   │  │ ● Farmácia       -15.00€   Saúde      │  │
│                   │  └───────────────────────────────────────┘  │
└───────────────────┴─────────────────────────────────────────────┘
```

**Elementos:**
- 3 summary cards no topo (total transações, receitas, despesas)
- Botões de ação: "Nova Transação" e "Importar CSV"
- Filter pills: tipo (todas/receitas/despesas), categoria, período
- Transações agrupadas por data (micro-label como separador)
- Cada TransactionRow: ícone categoria, descrição, valor, comerciante
- Paginação infinita ou botão "Carregar mais"

---

## W05 — Página de Orçamentos

```
┌─────────────────────────────────────────────────────────────────┐
│ Sidebar          │  ORÇAMENTOS                                  │
│                   │                                              │
│                   │  Período: (Jan) (Fev) (Mar) (Abr) ...       │
│                   │                                              │
│                   │  [+ Novo Orçamento]                          │
│                   │                                              │
│                   │  ┌───────────────────────────────────────┐  │
│                   │  │ ● Restauração                          │  │
│                   │  │   120 / 200 EUR                        │  │
│                   │  │   ████████████░░░░░░░░░  60%           │  │
│                   │  │                               [Apagar] │  │
│                   │  └───────────────────────────────────────┘  │
│                   │                                              │
│                   │  ┌───────────────────────────────────────┐  │
│                   │  │ ● Transportes                          │  │
│                   │  │   85 / 150 EUR                         │  │
│                   │  │   ██████████░░░░░░░░░░░  57%           │  │
│                   │  │                               [Apagar] │  │
│                   │  └───────────────────────────────────────┘  │
│                   │                                              │
│                   │  ┌───────────────────────────────────────┐  │
│                   │  │ ● Entretenimento                       │  │
│                   │  │   45 / 50 EUR                          │  │
│                   │  │   ██████████████████░░░  90%  ⚠        │  │
│                   │  │                               [Apagar] │  │
│                   │  └───────────────────────────────────────┘  │
│                   │                                              │
│                   │  RESUMO DO MÊS                               │
│                   │  Total orçamentado: 400 EUR                  │
│                   │  Total gasto: 250 EUR (63%)                  │
└───────────────────┴─────────────────────────────────────────────┘
```

**Elementos:**
- Filter pills de meses no topo
- Botão "Novo Orçamento" abre formulário inline
- Cards por categoria: ícone, nome, valor gasto/limite, barra de progresso
- Barra muda de cor quando > 80% (warning) ou > 100% (danger)
- Botão de eliminar em cada card
- Resumo do mês no fundo

---

## W06 — Página de Perfil

```
┌─────────────────────────────────────────────────────────────────┐
│ Sidebar          │  PERFIL                                      │
│                   │                                              │
│                   │  ┌───────────────────────────────────────┐  │
│                   │  │  DADOS PESSOAIS                        │  │
│                   │  │                                        │  │
│                   │  │  Nome        ( João Silva            ) │  │
│                   │  │  Email       ( joao@email.com        ) │  │
│                   │  │  Rendimento  ( 1500                  ) │  │
│                   │  │  Moeda       ▼ EUR                     │  │
│                   │  │                                        │  │
│                   │  │  [ Guardar Alterações ]                │  │
│                   │  └───────────────────────────────────────┘  │
│                   │                                              │
│                   │  ┌───────────────────────────────────────┐  │
│                   │  │  CONTAS BANCÁRIAS          [+ Nova]    │  │
│                   │  │                                        │  │
│                   │  │  ● Caixa Geral                         │  │
│                   │  │    Conta Corrente — 1 500,00 EUR       │  │
│                   │  │                                        │  │
│                   │  │  ● NovoBanco                           │  │
│                   │  │    Poupança — 2 000,00 EUR             │  │
│                   │  │                                        │  │
│                   │  └───────────────────────────────────────┘  │
│                   │                                              │
│                   │  ┌───────────────────────────────────────┐  │
│                   │  │  PREFERÊNCIAS                          │  │
│                   │  │                                        │  │
│                   │  │  Tema: (Claro) (Escuro) (Sistema)      │  │
│                   │  │                                        │  │
│                   │  └───────────────────────────────────────┘  │
└───────────────────┴─────────────────────────────────────────────┘
```

**Elementos:**
- Card "Dados Pessoais": inputs editáveis + botão guardar
- Card "Contas Bancárias": lista de contas com botão "Nova"
- Card "Preferências": toggle de tema (claro/escuro)
- Cada conta mostra: nome do banco, tipo, saldo

---

## W07 — Wizard de Onboarding (Modal)

```
┌──────────────────────────────────────────────────┐
│              Overlay (bg escuro + blur)           │
│                                                  │
│        ┌────────────────────────────────┐        │
│        │  Passo 1 de 4          Saltar  │        │
│        │  ████░░░░░░░░░░░░  25%         │        │
│        │                                │        │
│        │          👋                    │        │
│        │                                │        │
│        │   Bem-vindo ao FinTwin!        │        │
│        │                                │        │
│        │   O teu assistente financeiro  │        │
│        │   inteligente. Vamos configurar│        │
│        │   a tua conta em 4 passos.     │        │
│        │                                │        │
│        │   ✓ Conecta a tua conta        │        │
│        │   ✓ Regista uma transação      │        │
│        │   ✓ Define um orçamento        │        │
│        │   ✓ Acompanha o teu progresso  │        │
│        │                                │        │
│        │   [      Começar      ]        │        │
│        │                                │        │
│        └────────────────────────────────┘        │
│                                                  │
└──────────────────────────────────────────────────┘
```

**4 Passos do Wizard:**

| Passo | Conteúdo | Campos |
|:-----:|----------|--------|
| 1 | Boas-vindas + overview | Nenhum (apenas CTA) |
| 2 | Criar conta bancária | Nome do banco, tipo, saldo |
| 3 | Primeira transação | Descrição, valor, categoria |
| 4 | Definir orçamento | Categoria, limite mensal |

**Interações:**
- Barra de progresso no topo (25% por passo)
- Botão "Saltar" em cada passo
- Cada passo tem: "Saltar" (secundário) + "Próximo/Concluir" (primário)
- Ao concluir, marca `localStorage.fintwin_onboarded = 1`
