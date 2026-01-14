Kubernetes (o K8s) è un motore _open source_ per l'orchestrazione di container. Serve a distribuire, scalare e gestire applicazioni containerizzate in modo automatizzato .

- **Il problema:** Quando il numero di componenti di un'applicazione (container) cresce o si gestiscono migliaia di server, la gestione manuale diventa impossibile. Serve un meccanismo automatico .
- **La soluzione:** K8s astrae l'infrastruttura sottostante. Non importa quanti nodi (computer) ci siano; il sistema li vede come un'unica macchina e gestisce le risorse automaticamente .

# Architettura di Kubernetes

Il sistema si divide in due parti principali :

- **Control Plane (Master)** - Il "cervello" che controlla il sistema:
	- **API Server:** Il punto di accesso che abilita la comunicazione tra i componenti e l'amministratore .
	- **Scheduler:** Assegna i componenti da eseguire (pod) ai nodi worker disponibili.
	- **Etcd:** Un database che conserva la configurazione e lo stato del cluster.
	- **Controller Manager:** Gestisce le funzioni a livello di cluster, come la replicazione dei componenti e la gestione dei guasti dei nodi.
- **Worker Nodes** - I "muscoli" dove girano le applicazioni:
	- **Kubelet:** Un agente (demone) che comunica con l'API server e gestisce i container sul proprio nodo .
	- **Kube-proxy:** Gestisce il traffico di rete e il bilanciamento del carico (load balancing) tra i componenti .
	- **Container Runtime:** Il software che esegue effettivamente i container (es. [[Docker]]).

# Funzionalità principali

Kubernetes garantisce che lo stato dell'applicazione corrisponda sempre a quello desiderato dall'utente (descritto in file YAML).

- **Self-healing:** Se un componente si blocca, K8s lo riavvia automaticamente.
- **Scaling:** Aumenta o diminuisce automaticamente il numero di copie dell'applicazione in base al carico.
- **Aggiornamenti:** Permette di aggiornare le app eseguendo nuove istanze in parallelo e rimuovendo quelle vecchie solo se tutto funziona.

# Il Flusso di Deployment

Per eseguire un'app:

1. Lo sviluppatore crea un **App descriptor** (file YAML o JSON) che descrive cosa vuole (es. "Voglio 5 copie di questo container").
2. L'**API Server** riceve la richiesta.
3. Lo **Scheduler** decide su quali nodi mettere i container.
4. Il **Kubelet** sui nodi scelti istruisce il runtime di scaricare l'immagine (dal registry) ed eseguirla.

# Strumenti pratici: Minikube e Kubectl

- **Minikube:** È uno strumento per creare un cluster Kubernetes locale (sul proprio PC), ideale per test e sviluppo. Esegue K8s all'interno di una macchina virtuale (VM) o tramite Docker.
    - Comandi utili: `minikube start` (avvia), `minikube dashboard` (interfaccia grafica), `minikube delete` . 
- **Kubectl:** È l'interfaccia a riga di comando (CLI) per interagire con il cluster Kubernetes.
    - Comandi utili: `kubectl cluster-info` (info sul cluster), `kubectl get no` (lista nodi), `kubectl describe` (dettagli risorse) .

In sintesi, il flusso di lavoro descritto è: lo sviluppatore scrive un "descrittore" dell'app (file YAML), lo invia al Master tramite l'API Server, e il Master ordina ai Worker Nodes di scaricare le immagini ed eseguire i container necessari.