Il Pod è l'**unità atomica** (la più piccola) che si può creare e gestire in Kubernetes.

- **Non gestisci container:** In K8s non distribuiamo direttamente dei container (come faresti con Docker puro), ma distribuiamo Pod che _contengono_ container.
- **Contenuto:** Un Pod può contenere uno o più container (ad esempio, un container principale per l'app e uno "sidecar" di supporto).
- **Astrazione:** Pensiamo al Pod come a una "macchina logica" (un host virtuale) con il proprio indirizzo IP, hostname e processi .


> [!NOTE] Perché non usare semplicemente i container singoli?
> - **Raggruppamento logico:** Le applicazioni complesse sono spesso composte da più processi che devono comunicare strettamente (es. via _localhost_ o condividendo file locali) .
> - **Condivisione risorse:** Tutti i container dentro lo stesso Pod condividono lo stesso **Network Namespace** (stesso IP e stesse porte) e possono comunicare tramite IPC (Inter-Process Communication) .
> - **Isolamento:** Se usassimo solo container sparsi, dovremmo mappare manualmente le porte per evitare conflitti. Il Pod risolve questo problema assegnando a ciascun gruppo un IP dedicato .

==Kubernetes utilizza un modello di "Flat Network" (rete piatta senza NAT tra i pod), dove ogni Pod ha il suo IP unico. Tutti i Pod possono comunicare con tutti gli altri Pod direttamente, senza bisogno di traduzione degli indirizzi (NAT).==

# Creazione e Gestione

==I Pod vengono definiti tramite file **YAML** (o JSON) chiamati "manifest"==, che contengono tre sezioni principali :

1. **Metadata:** Nome del pod, etichette (labels), namespace.
2. **Spec:** La descrizione desiderata (quali container usare, quali immagini Docker, quali porte).  
3. **Status:** Lo stato attuale (gestito automaticamente da K8s, non modificabile dall'utente nel file).

Comandi essenziali:

- `kubectl create -f file.yaml`: Crea la risorsa definita nel file.
- `kubectl get pods`: Elenca i pod attivi.
- `kubectl port-forward <pod> <porta-locale>:<porta-pod>`: Permette di accedere a un Pod dal proprio computer locale per test e debug (es. mappando la porta 8888 locale alla 8080 del pod)

# Organizzazione e Scheduling (Labels & Selectors)

Man mano che i Pod aumentano, serve un modo per organizzarli.

- **Labels (Etichette):** ==Sono coppie chiave-valore (es. env: prod, creation_method: manual) attaccate ai Pod per categorizzarli .==
- **Node Selector:** ==Possiamo dire a Kubernetes di eseguire un Pod solo su specifici nodi che hanno una certa etichetta.==
    - _Esempio:_ Se abbiamo un nodo con GPU, gli diamo l'etichetta `gpu=true`. Poi, nella definizione del Pod, usiamo `nodeSelector: gpu: "true"` per forzare l'esecuzione su quel nodo .

# Namespace

I Namespace servono a separare logicamente le risorse all'interno dello stesso cluster fisico (es. separare l'ambiente di "sviluppo" da quello di "produzione" o separare team diversi) .

# Ciclo di vita (Cancellazione)

Quando cancelliamo un Pod (`kubectl delete po`), K8s invia un segnale `SIGTERM` ai processi per permettere loro di chiudersi ordinatamente (graceful shutdown). Se non si chiudono entro un timeout (default 30s), vengono forzati con `SIGKILL` .

# Replica e controllo

K8s riavvia automaticamente un container se crasha, ma a volte un'app può bloccarsi (deadlock, loop infinito) senza crashare. La soluzione è che K8s controlla periodicamente la salute dell'app dall'esterno tramite sonde (probes), se la sonda fallisce viene riavviato il container. Elenchiamo di seguito i vari meccanismi di probes:

- _HTTP Get:_ Richiede una risorsa web; se fallisce, il container viene riavviato.
- _TCP Socket:_ Tenta di aprire una connessione TCP.
- _Exec:_ Esegue un comando arbitrario dentro il container.

## ReplicaSet (RS)

==Il ReplicaSet è l'oggetto fondamentale per garantire che un certo numero di copie (repliche) di un Pod sia sempre attivo. Se un nodo cade o un Pod viene cancellato, il RS ne crea uno nuovo altrove per mantenere il numero desiderato==. Il RS è strutturato in 3 parti:

1. **Pod Selector:** Seleziona i Pod da gestire tramite etichette (labels) (es. `app=kubia`).
2. **Replica Count:** Il numero desiderato di Pod.
3. **Pod Template:** Il modello usato per creare nuovi Pod quando necessario.

==Il RS controlla i Pod basandosi sulle label. Se cambiamo la label di un Pod in esecuzione, questo esce dal controllo del RS, il quale ne creerà subito uno nuovo per rimpiazzarlo. Quindi non K8s consente cambiare il label selector.==

Invece è possibile modificare il _template_ del RS (ad esempio se cambiamo l'etichetta o l'immagine del container) che non aggiorna i Pod esistenti, ma solo quelli nuovi. Per aggiornare tutto, bisogna cancellare i vecchi Pod. Per aumentare o diminuire i Pod, basta cambiare il valore `replicas` (es. `kubectl scale`).

## DaemonSet

==A differenza del ReplicaSet, il DaemonSet serve per eseguire **un Pod su ogni nodo** (o su nodi specifici) del cluster== (ad esempio processi di sistema come raccoglitori di log o monitoraggio delle risorse). ==Se si aggiunge un nuovo nodo al cluster, il DS aggiunge automaticamente il Pod su quel nodo. Se qualcuno cancella manualmente un Pod del DaemonSet, Kubernetes lo ricrea automaticamente sullo stesso nodo (o su un nodo idoneo)==. È possibile limitare il DS a specifici nodi usando `nodeSelector` (es. eseguire solo su nodi con disco SSD).

> [!NOTE] Non fa nulla se un nodo va giù
> Se un nodo diventa _NotReady_ o si spegne Il DaemonSet **non sposta il Pod su un altro nodo** aspetta che il nodo torni disponibile.

## Job

==Il Job gestisce task che devono **terminare** una volta completato il lavoro== (a differenza di RS e DS che mantengono i processi attivi all'infinito). Se il processo fallisce (codice di uscita errore), il Job può riavviare il container o meno, in base alla `restartPolicy` (es. `OnFailure` o `Never`). Una volta finito con successo, il Pod non viene riavviato.

## CronJob

==Il CronJob serve per eseguire dei Job in momenti specifici o intervalli regolari (basato sul sistema cron di Unix/Linux). Il CronJob crea un oggetto Job, che a sua volta crea i Pod==. Usa il formato standard a 5 campi (minuti, ore, giorno del mese, mese, giorno della settimana) per definire la schedulazione (es. `*/4 * * * *` per ogni 4 minuti).