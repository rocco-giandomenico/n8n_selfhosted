# n8n Self-Hosted con Docker Compose

Setup completo per n8n self-hosted con database Postgres, supporto MCP (Model Context Protocol) e rotazione log automatica.

## Prerequisiti

- [Docker](https://docs.docker.com/get-docker/) installato e funzionante.
- Docker Compose (incluso nelle versioni recenti di Docker Desktop/Plugin).

## Installazione

1.  **Configurazione Ambiente**

    ```bash
    cp .env.sample .env
    ```

2.  **Per ragioni di sicurezza, modifica i seguenti valori:**
    
    *   `POSTGRES_PASSWORD`: Imposta una password robusta per il database.
    *   `N8N_BASIC_AUTH_PASSWORD`: Imposta la password per accedere a n8n.
    *   `N8N_ENCRYPTION_KEY`: Genera una nuova chiave segreta. **Non usare quella di esempio!**
    
    Puoi generare una chiave sicura con questo comando (se hai Python):
    ```bash
    python -c "import secrets; print(secrets.token_urlsafe(32))"
    ```
    Oppure usando OpenSSL:
    ```bash
    openssl rand -hex 32
    ```

3.  **Avvio**
    Esegui i container in background:
    ```bash
    docker compose up -d
    ```

## Utilizzo

1.  Apri il browser e vai su: `http://localhost:5678`
2.  Inserisci user e password definiti in `N8N_BASIC_AUTH_USER` e `N8N_BASIC_AUTH_PASSWORD`.

## Funzionalità Incluse

*   **Database Postgres dedicato**
*   **MCP Abilitato**
*   **Basic Auth**
*   **Restart Automatico**
*   **Rotazione Log**
    *   Driver: `json-file`
    *   Dimensione massima per file: `10m` (10 Megabyte)
    *   Numero massimo di file ruotati: `3`

## Backup

Per un backup completo, assicurati di salvare:
1.  Il file `.env` (contiene la chiave di cifratura!).
2.  Le cartelle locali create nel progetto:
    *   `./n8n_data`: Contiene i workflow e le credenziali di n8n.
    *   `./postgres_data`: Contiene il database Postgres.

## Aggiornamento


*   **n8n**: Ultima versione disponibile (`latest`).
*   **Postgres (16-alpine)**: Solo aggiornamenti di sicurezza.

Procedura:

```bash
docker compose down
```

```bash
docker compose pull
```

```bash
docker compose up -d
```

