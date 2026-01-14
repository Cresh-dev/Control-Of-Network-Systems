
_Tag:_ #docker #images

---
Le immagini in docker sono uno strumento fondamentale che hanno un forte legame con i [[Container]], esse contengono tutti i file (di configurazione, binari, librerie ...) necessari ad avviare un container. Possiamo vedere il tutto da un punto di vista più pragmatico immaginandoci che le immagini sono delle classi e i container sono delle istanze di queste classi.

## Punti Chiave

- **Immutabilità:** Le immagini sono immutabili, una volta creata un immagine non si può modificare
- **Composizione delle immagini:** Le immagini sono composte da layers e ognuno di questi livelli rappresentano un insieme di cambiamenti al file system che aggiungono, rimuovono o modificano i file

## Info utili

Il [docker hub](https://hub.docker.com/) è uno store dove possiamo trovare varie immagini già fatte e distribuite. Possiamo trovare una grandissima varietà di immagini e scaricare quella che ci serve.

## Gestione delle Immagini

- **Scaricare un'immagine**: `docker pull ubuntu`
- **Elencare le immagini disponibili**: `docker images`
- **Rimuovere un'immagine**: `docker rmi <image_id>`