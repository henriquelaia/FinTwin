# FinTwin — Wireframes (Sprint 1)

## Convencoes

```
┌─────────┐  Caixa = container/card
│         │
└─────────┘

[  Botao  ]  Botao clicavel

( campo )    Input field

── ── ── ──  Separador/divider

▼            Dropdown/select

●            Icone/avatar
```

---

## W01 — Pagina de Login

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
│              │   Nao tens conta?    │            │
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
- Botao primario "Entrar" (btn-primary, full-width)
- Link para pagina de registo
- Background com gradiente da cor brand

**Interacoes:**
- Enter submete o formulario
- Erro de login mostra mensagem abaixo do formulario
- Apos login, redireciona para Dashboard

---

## W02 — Pagina de Registo

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
│              │   Ja tens conta?     │            │
│              │   Entrar             │            │
│              │                      │            │
│              └──────────────────────┘            │
│                                                  │
└──────────────────────────────────────────────────┘
```

**Elementos:**
- Campos: nome, email, password (obrigatorios), rendimento e moeda (opcionais)
- Dropdown de moeda (EUR, USD, GBP)
- Botao "Criar Conta"
- Link para login

---

## W03 — Dashboard

```
┌─────────────────────────────────────────────────────────────────┐
│ ● Sidebar        │  Header: glass panel + search + dark toggle │
│                   │─────────────────────────────────────────────│
│ ▸ Dashboard      │                                              │
│   Transacoes     │  BOM DIA, JOAO                               │
│   Orcamentos     │                                              │
│   Perfil         │  ┌──────────────┐ ┌──────────┐ ┌──────────┐ │
│                   │  │ SALDO TOTAL  │ │ RECEITAS │ │ DESPESAS │ │
│                   │  │              │ │          │ │          │ │
│                   │  │  3 500,00 €  │ │ 1 500 € │ │  -820 € │ │
│                   │  │              │ │  ↑ 12%   │ │  ↓ 5%   │ │
│                   │  └──────────────┘ └──────────┘ └──────────┘ │
│   ── ── ── ──    │                                              │
│                   │  ULTIMAS TRANSACOES          [Ver todas →]  │
│   Sair           │  ┌───────────────────────────────────────┐  │
│                   │  │ ● Continente    -45.30€   15 Jan     │  │
│                   │  │ ● Salario      +1500.00€  01 Jan     │  │
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
- Sidebar: navegacao principal, toggle dark mode, botao sair
- Greeting como micro-label ("BOM DIA, JOAO")
- 3 stat cards: saldo total (hero number), receitas, despesas
- Lista das 5 transacoes mais recentes (TransactionRow)
- Grafico pie chart de despesas por categoria
- Se utilizador novo: wizard de onboarding sobreposto

---

## W04 — Pagina de Transacoes

```
┌─────────────────────────────────────────────────────────────────┐
│ Sidebar          │  TRANSACOES                                  │
│                   │                                              │
│                   │  ┌──────────┐ ┌──────────┐ ┌──────────┐    │
│                   │  │ TOTAL    │ │ RECEITAS │ │ DESPESAS │    │
│                   │  │ 45 txns  │ │ 1 500 €  │ │  -820 €  │    │
│                   │  └──────────┘ └──────────┘ └──────────┘    │
│                   │                                              │
│                   │  [+ Nova Transacao]  [Importar CSV]          │
│                   │                                              │
│                   │  Filtros: (Todas) (Receitas) (Despesas)      │
│                   │           ▼ Categoria   ▼ Periodo            │
│                   │                                              │
│                   │  HOJE — 22 MAR 2026                          │
│                   │  ┌───────────────────────────────────────┐  │
│                   │  │ ● Supermercado    -45.30€  Continente │  │
│                   │  └───────────────────────────────────────┘  │
│                   │  ┌───────────────────────────────────────┐  │
│                   │  │ ● Almoco          -8.50€   Restaurante│  │
│                   │  └───────────────────────────────────────┘  │
│                   │                                              │
│                   │  ONTEM — 21 MAR 2026                         │
│                   │  ┌───────────────────────────────────────┐  │
│                   │  │ ● Uber            -6.20€   Transportes│  │
│                   │  └───────────────────────────────────────┘  │
│                   │  ┌───────────────────────────────────────┐  │
│                   │  │ ● Farmacia       -15.00€   Saude      │  │
│                   │  └───────────────────────────────────────┘  │
└───────────────────┴─────────────────────────────────────────────┘
```

**Elementos:**
- 3 summary cards no topo (total transacoes, receitas, despesas)
- Botoes de acao: "Nova Transacao" e "Importar CSV"
- Filter pills: tipo (todas/receitas/despesas), categoria, periodo
- Transacoes agrupadas por data (micro-label como separador)
- Cada TransactionRow: icone categoria, descricao, valor, comerciante
- Paginacao infinita ou botao "Carregar mais"

---

## W05 — Pagina de Orcamentos

```
┌─────────────────────────────────────────────────────────────────┐
│ Sidebar          │  ORCAMENTOS                                  │
│                   │                                              │
│                   │  Periodo: (Jan) (Fev) (Mar) (Abr) ...       │
│                   │                                              │
│                   │  [+ Novo Orcamento]                          │
│                   │                                              │
│                   │  ┌───────────────────────────────────────┐  │
│                   │  │ ● Restauracao                          │  │
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
│                   │  RESUMO DO MES                               │
│                   │  Total orcamentado: 400 EUR                  │
│                   │  Total gasto: 250 EUR (63%)                  │
└───────────────────┴─────────────────────────────────────────────┘
```

**Elementos:**
- Filter pills de meses no topo
- Botao "Novo Orcamento" abre formulario inline
- Cards por categoria: icone, nome, valor gasto/limite, barra de progresso
- Barra muda de cor quando > 80% (warning) ou > 100% (danger)
- Botao de eliminar em cada card
- Resumo do mes no fundo

---

## W06 — Pagina de Perfil

```
┌─────────────────────────────────────────────────────────────────┐
│ Sidebar          │  PERFIL                                      │
│                   │                                              │
│                   │  ┌───────────────────────────────────────┐  │
│                   │  │  DADOS PESSOAIS                        │  │
│                   │  │                                        │  │
│                   │  │  Nome        ( Joao Silva            ) │  │
│                   │  │  Email       ( joao@email.com        ) │  │
│                   │  │  Rendimento  ( 1500                  ) │  │
│                   │  │  Moeda       ▼ EUR                     │  │
│                   │  │                                        │  │
│                   │  │  [ Guardar Alteracoes ]                │  │
│                   │  └───────────────────────────────────────┘  │
│                   │                                              │
│                   │  ┌───────────────────────────────────────┐  │
│                   │  │  CONTAS BANCARIAS          [+ Nova]    │  │
│                   │  │                                        │  │
│                   │  │  ● Caixa Geral                         │  │
│                   │  │    Conta Corrente — 1 500,00 EUR       │  │
│                   │  │                                        │  │
│                   │  │  ● NovoBanco                           │  │
│                   │  │    Poupanca — 2 000,00 EUR             │  │
│                   │  │                                        │  │
│                   │  └───────────────────────────────────────┘  │
│                   │                                              │
│                   │  ┌───────────────────────────────────────┐  │
│                   │  │  PREFERENCIAS                          │  │
│                   │  │                                        │  │
│                   │  │  Tema: (Claro) (Escuro) (Sistema)      │  │
│                   │  │                                        │  │
│                   │  └───────────────────────────────────────┘  │
└───────────────────┴─────────────────────────────────────────────┘
```

**Elementos:**
- Card "Dados Pessoais": inputs editaveis + botao guardar
- Card "Contas Bancarias": lista de contas com botao "Nova"
- Card "Preferencias": toggle de tema (claro/escuro)
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
│        │   ✓ Regista uma transacao      │        │
│        │   ✓ Define um orcamento        │        │
│        │   ✓ Acompanha o teu progresso  │        │
│        │                                │        │
│        │   [      Comecar      ]        │        │
│        │                                │        │
│        └────────────────────────────────┘        │
│                                                  │
└──────────────────────────────────────────────────┘
```

**4 Passos do Wizard:**

| Passo | Conteudo | Campos |
|:-----:|----------|--------|
| 1 | Boas-vindas + overview | Nenhum (apenas CTA) |
| 2 | Criar conta bancaria | Nome do banco, tipo, saldo |
| 3 | Primeira transacao | Descricao, valor, categoria |
| 4 | Definir orcamento | Categoria, limite mensal |

**Interacoes:**
- Barra de progresso no topo (25% por passo)
- Botao "Saltar" em cada passo
- Cada passo tem: "Saltar" (secundario) + "Proximo/Concluir" (primario)
- Ao concluir, marca `localStorage.fintwin_onboarded = 1`
