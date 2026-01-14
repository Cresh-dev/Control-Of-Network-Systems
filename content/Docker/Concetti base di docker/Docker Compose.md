
_Tag:_ #docker #compose

---
Una ottima pratica è quella di avere un [[Container]] per ogni singolo servizio, però possiamo anche avere un contenitore che fa più cose. Con docker compose posso definire tutti i miei contenitori e le loro configurazioni in un singolo file YAML e con un semplice comando `docker compose up` posso avviare il tutto.

## Componenti del docker compose

- **File:** docker-compose.yml è il file principale dove viene definita tutta l'architettura dell'applicazione
- **Servizi:** ogni container viene descritto come un servizio all'interno del file yaml e può includere configurazioni come :
	- **Immagini:** [[Immagini Docker]] con cui si avvia il container
	- **Build:** indica che l'immagine deve essere costruita da un docker file
	- **Port:** le porte esposte del container verso l'esterno
	- **Environment:** variabili d'ambiente per il container
	- **Volume:** volumi montati per persistenza dei dati
	- **Network:** le reti a cui il container è collegato
- **Volumi:** sono utilizzati per la persistenza dei dati ai riavvii dei container
- **Reti:** reti a cui il container è collegato

Esempio di `docker-compose.yml` per un'app con **Nginx** e **PostgreSQL**:

```docker
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8080:80"

  db:
    image: postgres
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
```

Per avviare i servizi: `docker-compose up -d`
Per fermarli: `docker-compose down`