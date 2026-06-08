_Tag:_ #container #docker

---
I container sono processi isolati che ci permettono di eseguire le componenti (database, front-end, back-end ...) che costituiscono un applicazione. Il tutto viene fatto in un ambiente isolato da qualsiasi altra cosa sulla nostra macchina.

## Caratteristiche Principali

- **Autonomo:** Ogni container ha il necessario per funzionare senza la pre-installazione di dipendenze
- **Isolato:** per aumentare la sicurezza della nostra applicazione i container sono completamente isolati quindi hanno una minima influenza sull'host
- **Indipendente:** ogni container è indipendente dall'altro
- **Portabile:** i container possono essere avviati da qualsiasi parte

## Differenze Importanti

- **Container vs macchine virtuali:** Andare ad utilizzare una vm per l'esecuzione di un'app è troppo dispendioso per l'host quindi si ha una scelta preferenziale del container per la sua leggerezza e immediatezza.

## I dati all'interno dei containers

Quando creiamo un container da un'[[Immagini Docker|immagine]], tutto ciò che è presente nell'immagine viene trattato come di sola lettura e sopra di esso viene sovrapposto un nuovo livello di lettura/scrittura.

![[Pasted image 20250324165039.png]]

Spesso, le nostre applicazioni producono dati che dobbiamo conservare in modo sicuro (ad esempio, dati di database, dati caricati dagli utenti, ecc.) anche se i contenitori vengono distrutti e ricreati. Fortunatamente, Docker (e i contenitori in generale) hanno una funzionalità per gestire questo caso d'uso chiamata volumes e mounts. 

![[Pasted image 20250324165236.png]]

I volumes e i mounts ci consentono di specificare una posizione in cui i dati devono persistere oltre il ciclo di vita di un singolo contenitore. I dati possono risiedere in una posizione gestita da Docker (mount del volume), una posizione nel file system host (mount di bind) o nella memoria (mount tmpfs, non illustrato).

## Gestione dei Container

- **Creare ed eseguire un container**: `docker run ubuntu`. Questo comando scarica l'immagine di Ubuntu e avvia un container.
- **Eseguire un container in modalità interattiva**: `docker run -it ubuntu /bin/bash`. Ti permette di entrare in una shell del container.
- **Mostrare i container in esecuzione**: `docker ps`
- **Mostrare tutti i container (anche quelli fermati)**: `docker ps -a`
- **Ferma un container**: `docker stop <container_id>`
- **Rimuovere un container**: `docker rm <container_id>`

## Gestione dei Volumi e Persistenza dei Dati

- **Creare un volume**: `docker volume create mio-volume`
- **Montare un volume in un container**: `docker run -v mio-volume:/data -it ubuntu`. Questo comando monta il volume `mio-volume` nella directory `/data` del container.

## Esposizione di Porte

- **Eseguire un container esponendo una porta**: `docker run -p 8080:80 nginx`. Espone la porta `80` del container sulla porta `8080` dell'host.