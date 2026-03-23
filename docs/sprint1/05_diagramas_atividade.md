# FinTwin — Diagramas de Atividade (Sprint 1)

## DA01 — Fluxo de Onboarding (Primeiro Acesso)

```mermaid
flowchart TD
    START((Início)) --> REG[Utilizador faz registo]
    REG --> JWT[Sistema emite JWT]
    JWT --> DASH[Redireciona para Dashboard]
    DASH --> CHECK{Tem contas<br/>bancárias?}

    CHECK -->|Não| WIZ[Abre Wizard de Onboarding]
    CHECK -->|Sim| SHOW[Mostra Dashboard normal]

    WIZ --> S1[Passo 1: Boas-vindas]
    S1 --> S1A{Clica Começar<br/>ou Saltar?}
    S1A -->|Começar| S2[Passo 2: Criar Conta Bancária]
    S1A -->|Saltar| S2

    S2 --> S2A{Preenche dados<br/>da conta?}
    S2A -->|Sim| S2B[Cria conta via API]
    S2A -->|Saltar| S3[Passo 3: Primeira Transação]
    S2B --> S3

    S3 --> S3A{Preenche<br/>transação?}
    S3A -->|Sim| S3B[Cria transação via API]
    S3A -->|Saltar| S4[Passo 4: Definir Orçamento]
    S3B --> S4

    S4 --> S4A{Define<br/>orçamento?}
    S4A -->|Sim| S4B[Cria orçamento via API]
    S4A -->|Saltar| FIN[Marca onboarding como completo]
    S4B --> FIN

    FIN --> LS[localStorage: fintwin_onboarded = 1]
    LS --> SHOW

    SHOW --> END((Fim))
```

---

## DA02 — Fluxo de Importação CSV

```mermaid
flowchart TD
    START((Início)) --> OPEN[Utilizador clica Importar CSV]
    OPEN --> MODAL[Modal de importação abre]
    MODAL --> SEL[Seleciona conta de destino]
    SEL --> FILE{Seleciona ou<br/>arrasta ficheiro}

    FILE --> VALID{Ficheiro é<br/>.csv?}
    VALID -->|Não| ERR1[Mensagem: Apenas CSV aceites]
    ERR1 --> FILE
    VALID -->|Sim| READ[FileReader lê o ficheiro]

    READ --> PARSE[Parse CSV: extrair linhas]
    PARSE --> PREVIEW[Mostrar pré-visualização<br/>5 primeiras linhas + total]

    PREVIEW --> CONF{Utilizador confirma<br/>importação?}
    CONF -->|Cancelar| CLOSE[Fecha modal]
    CONF -->|Confirmar| UPLOAD[Enviar ficheiro para API]

    UPLOAD --> API[Backend: csv.DictReader]
    API --> LOOP{Mais linhas<br/>para processar?}

    LOOP -->|Sim| PARSELN[Parse: date, description, amount, merchant]
    PARSELN --> CATEG[Categorizar automaticamente]
    CATEG --> INSERT[INSERT transação na DB]
    INSERT --> LOOP

    LOOP -->|Não| FLUSH[Flush: commit todas as transações]
    FLUSH --> RESP[Retorna: N transações importadas]
    RESP --> SUCCESS[Modal fecha + lista atualizada]
    SUCCESS --> END((Fim))

    CLOSE --> END
```

---

## DA03 — Fluxo de Autenticação (Login + Acesso Protegido)

```mermaid
flowchart TD
    START((Início)) --> ACCESS[Utilizador acede a uma página]
    ACCESS --> PROT{Página é<br/>protegida?}

    PROT -->|Não| RENDER[Renderizar página pública]
    RENDER --> END((Fim))

    PROT -->|Sim| TOKEN{JWT existe no<br/>localStorage?}
    TOKEN -->|Não| LOGIN[Redirecionar para /login]

    TOKEN -->|Sim| VERIFY[Enviar request com<br/>Authorization: Bearer JWT]
    VERIFY --> VALID{Token<br/>válido?}

    VALID -->|Sim| AUTH[Extrair user_id do token]
    AUTH --> DATA[Carregar dados do utilizador]
    DATA --> PAGE[Renderizar página protegida]
    PAGE --> END

    VALID -->|Não| CLEAR[Limpar token do localStorage]
    CLEAR --> LOGIN

    LOGIN --> FORM[Utilizador preenche email + password]
    FORM --> SUBMIT[POST /api/v1/auth/login]
    SUBMIT --> RATE{Rate limit<br/>OK?}

    RATE -->|Não| WAIT[Mensagem: Tenta mais tarde]
    WAIT --> FORM

    RATE -->|Sim| CRED{Credenciais<br/>corretas?}
    CRED -->|Não| ERRMSG[Mensagem: Email ou password incorretos]
    ERRMSG --> FORM

    CRED -->|Sim| NEWJWT[Gerar JWT com user_id]
    NEWJWT --> STORE[Armazenar JWT no localStorage]
    STORE --> REDIRECT[Redirecionar para página original]
    REDIRECT --> PAGE
```
