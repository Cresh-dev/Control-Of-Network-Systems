In un'architettura a microservizi, le applicazioni (client) devono conoscere l'indirizzo IP del server per comunicare. Tuttavia, in Kubernetes questo è difficile perché i [[Pod]] sono effimeri e quindi possono essere rimossi, scalati o fallire. Quando un pod viene ricreato, ottiene un **nuovo indirizzo IP**. Se abbiamo più copie dello stesso pod, dovrebbero condividere lo stesso punto di accesso, non avere IP sparsi. ==Un Service di Kubernetes è un'astrazione che definisce un singolo punto di ingresso stabile (un IP e una porta fissi) per un gruppo di pod==. Un service ha le seguenti caratteristiche:

- L'IP del Service non cambia mai finché il servizio esiste.  
- Il Service agisce come un proxy/maschera: il client chiama il Service, e il Service inoltra la richiesta a uno dei pod disponibili.
- I pod vengono associati al Service tramite i **Label Selector** (selettori di etichette).

# Service Discovery

Una volta creato un Service, come fanno gli altri pod a sapere quale IP chiamare? Kubernetes offre due metodi:

- **Variabili d'ambiente:** Quando un pod parte, K8s inietta variabili con gli indirizzi dei servizi attivi. 
	- Limite: il Service deve essere creato _prima_ del pod.
- **DNS (Metodo preferito):** K8s ha un server DNS interno (CoreDNS). Ogni Service ottiene un nome di dominio (FQDN) come `<nome-servizio>.<namespace>.svc.cluster.local`. I pod possono chiamare semplicemente il nome del servizio (es. `http://kubia`) e il DNS lo risolverà nell'IP del Service.

# Tipi di Service e Accesso Esterno

Abbiamo diversi modi per esporre i servizi, sia internamente che esternamente:

- **Internal cluster**: ==Il servizio ha un IP raggiungibile solo dall'interno del cluster. Serve per la comunicazione tra pod interni== (es. frontend che chiama backend). È ideale per la comunicazione tra le parti della nostra applicazione. Dall'esterno (internet) questo IP non è raggiungibile.
- **External Cluster**: Un Service solitamente instrada il traffico ai pod interni, ma può essere usato anche per puntare a server esterni (fuori dal cluster). Si crea un Service senza specificare il "pod selector". Si crea manualmente una risorsa `Endpoints` che contiene la lista degli IP e delle porte dei server esterni. In questo modo, i pod interni contattano il Service come se fosse locale, ma il traffico viene girato all'esterno.
- **NodePort:** Rende il servizio accessibile dall'esterno aprendo una porta specifica (es. 30123) su **tutti i nodi** del cluster. Si accede tramite `IP-del-Nodo:Porta`.
- **LoadBalancer:** Usato solitamente su cloud provider (AWS, Google, Azure). Richiede un Load Balancer esterno al provider, che fornisce un indirizzo IP pubblico dedicato per accedere al servizio.

> [!NOTE] Come funziona il Load Balancer
> Il cloud provider ci assegna un **indirizzo IP pubblico reale** (un numero fisso accessibile da internet). Quando un utente si collega a quell'IP, il Load Balancer riceve la richiesta e la smista automaticamente verso uno dei Nodi del cluster (usando internamente il NodePort visto prima).

## Ingress (Routing HTTP)

Quando si hanno molti servizi, esporli tutti con LoadBalancer o NodePort è costoso o complesso. Per questa motivazione abbiamo l'**ingress** che opera a livello applicativo (HTTP/Layer 7) e permette di usare un **singolo IP** per esporre molteplici servizi. Instrada il traffico basandosi sul nome host (es. `kubia.example.com`) o sul percorso (path). Richiede un **Ingress Controller** (come Nginx) per funzionare.

## Headless Service

Si crea impostando `ClusterIP: None`. Non fornisce un singolo IP per il bilanciamento. Invece, il DNS restituisce direttamente la lista degli IP di **tutti** i pod collegati. Quando togliamo l'IP centrale, cambia il comportamento del server DNS interno di Kubernetes, invece di restituire un solo indirizzo IP (quello del Service), il DNS restituisce una lista di **record A multipli**. Ogni record punta direttamente all'IP di un singolo **Pod** che compone il servizio in quel momento. È utile se il client deve comunicare direttamente con uno specifico pod o gestire il bilanciamento da sé.