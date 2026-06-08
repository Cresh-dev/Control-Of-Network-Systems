_Tag:_ #docker

---
Docker è uno strumento molto moderno e potente che ci premette di sviluppare app in dei container, quindi in un ambiente isolato dove possiamo effettuare le nostre sperimentazioni e i nostri sviluppi senza correre il rischio di andare a interferire con programmi o librerie che sono installate sul nostro sistema o ancor peggio andare a effettuare delle azioni irreversibili che possono distruggere il nostro sistema.

## Panoramica tecnologica

- [[Namespaces nei container]]
- [[cgroup]]
- [[Union mount file system]]

## Architettura di docker 

![[Pasted image 20250324162421.png]]

## Concetti di base

Prima di iniziare con Docker, è utile comprendere alcuni concetti fondamentali:
- **Container vs Virtual Machine**: Docker usa i container per isolare le applicazioni, a differenza delle macchine virtuali (VM), che creano ambienti più pesanti.
- **Immagini e container**: Un'immagine è un template per creare container. Un container è un'istanza di un'immagine in esecuzione.
- **Dockerfile**: Un file che definisce i passi per costruire un'immagine.
- **Registry (Docker Hub)**: Luogo in cui vengono archiviate e condivise le immagini Docker.

## Composizione di docker

- [[Immagini Docker]]
- [[Container]]
- [[Registri Docker]] 
- [[Docker Compose]]

## Costruzione delle immagini

![[Screenshot 2025-06-17 at 09.49.22.png]]

- [[Livelli dell'immagine docker]]
- [[Costruire e pubblicare un immagine docker]]
- [[Utilizzo della cache di build]]
- [[Costruzioni multi-fase]]

##  Comandi per container

![[Screenshot 2025-06-16 at 16.31.57.png]]
  
## Docker compose

**Docker Compose** è uno **strumento ufficiale di Docker** che permette di **definire e gestire applicazioni multi-container**. Viene usato per orchestrare e avviare più container Docker tramite **un solo file di configurazione YAML** (`docker-compose.yml`), in modo **coordinato e semplice**.

- [[Struttura di un file docker-compose.yml]]

## Volumi

I volumi sono **store di dati persistenti**, gestiti direttamente da Docker e archiviati in una cartella sul host (di solito `/var/lib/docker/volumes`). A differenza del filesystem interno ai container (effimero), i volumi conservano i dati anche se il container viene eliminato.

- [[Tipologie di volumi docker]]
- [[Comandi e gestione pratica dei volumi]]

## Networking

Quando avviamo un container Docker, Docker crea automaticamente una rete virtuale e connette il container ad essa. Questa rete consente:

- La comunicazione tra container
- La comunicazione con l'host
- L'accesso verso internet (e opzionalmente, dall’esterno verso il container)

Ogni container ha la propria **interfaccia di rete virtuale** (veth) e un indirizzo IP assegnato automaticamente.

- [[Tipi di network driver]]
- [[Comunicazione tra container]]
- [[Sicurezza e troubleshooting]]

### Comandi principali

|Funzione|Comando|
|---|---|
|Elenco reti|`docker network ls`|
|Crea rete bridge|`docker network create --driver bridge nome`|
|Ispeziona rete|`docker network inspect nome`|
|Connetti container a rete|`docker network connect rete container`|
|Container senza rete|`docker run --network none`|

## Dockerfile e containerfile

- [[Scrivere un dockerfile]]

## Best Practices e Ottimizzazione

- Usa **.dockerignore** per escludere file inutili (come `.git` o `node_modules`).
- Mantieni i container leggeri scegliendo immagini base ottimizzate (`alpine`, `slim`).
- Evita di eseguire processi come **root** per motivi di sicurezza.
- Usa **multi-stage builds** per ridurre le dimensioni delle immagini.
- **Etichetta le immagini** con versioni (`myapp:v1.0.0`) per evitare ambiguità.

## Risorse utili

[RoadMap](https://roadmap.sh/docker)