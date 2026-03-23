# FinTwin — Diagramas de Atividade (Sprint 1)

## DA01 — Fluxo de Onboarding (Primeiro Acesso)

```mermaid
flowchart TD
    START((Inicio)) --> REG[Utilizador faz registo]
    REG --> JWT[Sistema emite JWT]
    JWT --> DASH[Redireciona para Dashboard]
    DASH --> CHECK{Tem contas<br/>bancarias?}

    CHECK -->|Nao| WIZ[Abre Wizard de Onboarding]
    CHECK -->|Sim| SHOW[Mostra Dashboard normal]

    WIZ --> S1[Passo 1: Boas-vindas]
    S1 --> S1A{Clica Comecar<br/>ou Saltar?}
    S1A -->|Comecar| S2[Passo 2: Criar Conta Bancaria]
    S1A -->|Saltar| S2

    S2 --> S2A{Preenche dados<br/>da conta?}
    S2A -->|Sim| S2B[Cria conta via API]
    S2A -->|Saltar| S3[Passo 3: Primeira Transacao]
    S2B --> S3

    S3 --> S3A{Preenche<br/>transacao?}
    S3A -->|Sim| S3B[Cria transacao via API]
    S3A -->|Saltar| S4[Passo 4: Definir Orcamento]
    S3B --> S4

    S4 --> S4A{Define<br/>orcamento?}
    S4A -->|Sim| S4B[Cria orcamento via API]
    S4A -->|Saltar| FIN[Marca onboarding como completo]
    S4B --> FIN

    FIN --> LS[localStorage: fintwin_onboarded = 1]
    LS --> SHOW

    SHOW --> END((Fim))
```

---

## DA02 — Fluxo de Importacao CSV

```mermaid
flowchart TD
    START((Inicio)) --> OPEN[Utilizador clica Importar CSV]
    OPEN --> MODAL[Modal de importacao abre]
    MODAL --> SEL[Seleciona conta de destino]
    SEL --> FILE{Seleciona ou<br/>arrasta ficheiro}

    FILE --> VALID{Ficheiro e<br/>.csv?}
    VALID -->|Nao| ERR1[Mensagem: Apenas CSV aceites]
    ERR1 --> FILE
    VALID -->|Sim| READ[FileReader le o ficheiro]

    READ --> PARSE[Parse CSV: extrair linhas]
    PARSE --> PREVIEW[Mostrar pre-visualizacao<br/>5 primeiras linhas + total]

    PREVIEW --> CONF{Utilizador confirma<br/>importacao?}
    CONF -->|Cancelar| CLOSE[Fecha modal]
    CONF -->|Confirmar| UPLOAD[Enviar ficheiro para API]

    UPLOAD --> API[Backend: csv.DictReader]
    API --> LOOP{Mais linhas<br/>para processar?}

    LOOP -->|Sim| PARSELN[Parse: date, description, amount, merchant]
    PARSELN --> CATEG[Categorizar automaticamente]
    CATEG --> INSERT[INSERT transacao na DB]
    INSERT --> LOOP

    LOOP -->|Nao| FLUSH[Flush: commit todas as transacoes]
    FLUSH --> RESP[Retorna: N transacoes importadas]
    RESP --> SUCCESS[Modal fecha + lista atualizada]
    SUCCESS --> END((Fim))

    CLOSE --> END
```

---

## DA03 — Fluxo de Autenticacao (Login + Acesso Protegido)

```mermaid
flowchart TD
    START((Inicio)) --> ACCESS[Utilizador acede a uma pagina]
    ACCESS --> PROT{Pagina e<br/>protegida?}

    PROT -->|Nao| RENDER[Renderizar pagina publica]
    RENDER --> END((Fim))

    PROT -->|Sim| TOKEN{JWT existe no<br/>localStorage?}
    TOKEN -->|Nao| LOGIN[Redirecionar para /login]

    TOKEN -->|Sim| VERIFY[Enviar request com<br/>Authorization: Bearer JWT]
    VERIFY --> VALID{Token<br/>valido?}

    VALID -->|Sim| AUTH[Extrair user_id do token]
    AUTH --> DATA[Carregar dados do utilizador]
    DATA --> PAGE[Renderizar pagina protegida]
    PAGE --> END

    VALID -->|Nao| CLEAR[Limpar token do localStorage]
    CLEAR --> LOGIN

    LOGIN --> FORM[Utilizador preenche email + password]
    FORM --> SUBMIT[POST /api/v1/auth/login]
    SUBMIT --> RATE{Rate limit<br/>OK?}

    RATE -->|Nao| WAIT[Mensagem: Tenta mais tarde]
    WAIT --> FORM

    RATE -->|Sim| CRED{Credenciais<br/>corretas?}
    CRED -->|Nao| ERRMSG[Mensagem: Email ou password incorretos]
    ERRMSG --> FORM

    CRED -->|Sim| NEWJWT[Gerar JWT com user_id]
    NEWJWT --> STORE[Armazenar JWT no localStorage]
    STORE --> REDIRECT[Redirecionar para pagina original]
    REDIRECT --> PAGE
```
