==Il nostro obiettivo tramite questo controllo è modellare il buffer dei chunk video, quindi noi siamo interessati effettivamente ai secondi di video immagazzinati nel buffer.== Possiamo esprimere di seguito la variazione del buffer tramite la derivata $\dot{t_f(t)}$ che cresce se la rete ($r$) è più veloce del consumo video ($l$):

$$\dot{t_f(t)} = \frac{r(t)}{l(t)} - o(t)$$

Dove:

- $r(t)$ è a velocità a cui i dati video arrivano fisicamente dal server al buffer del nostro dispositivo:$\frac{\Delta Data}{\Delta t}$;
- $l(t)$ è il livello dello stream;
- $o(t)$ è la velocità di riproduzione che di regola è un secondo di film su un secondo di tempo.

$r(t)$ si comporta come un rumore nel nostro sistema e il modello non è lineare perché noi vogliamo controllare $l(t)$ che si trova al denominatore, pertanto eseguiamo la linearizzazione in retroazione (_feedback linearization_). Usando un controllore proporzionale-integrale $\dot{t_f(t)}$ diventa $-k_p t_f(t) - k_i t_{fi}(t)$, dove $t_{fi}(t)$ è l'integrale dell'errore di $t_f(t)$, mentre $k_p$ e $k_i$ sono due parametri scelti. Il modello finale che si ottiene diventa:

$$l(t) = \frac{r(t)}{o(t) - k_p t_f(t) - k_i t_{fi}(t)}$$

1. **Se il buffer è troppo pieno:**
    - Vogliamo che il buffer scenda (cioè che $\dot{t}_f$ diventi negativo).
    - Poiché la qualità $l(t)$ è al **denominatore**, se aumentiamo $l(t)$ (scegliendo una qualità video più alta, es. 4K), il termine $\frac{r(t)}{l(t)}$ diventa più piccolo.
    - Quando il termine di ingresso $\frac{r(t)}{l(t)}$ diventa minore della velocità di riproduzione $o(t)$ (che è fissa a 1), il buffer inizia a svuotarsi. In pratica, stai scaricando "pochi secondi di video molto pesanti", quindi consumi il buffer accumulato mentre ci godiamo l'alta qualità.
2. **Se il buffer si sta svuotando (o è basso):**
    - Vogliamo che il buffer risalga rapidamente (cioè che $\dot{t}_f$ sia positivo e grande).
    - Dobbiamo **diminuire** la qualità $l(t)$. Un valore più piccolo al denominatore fa crescere il risultato della frazione.
    - In questo modo, con la stessa velocità di rete $r(t)$, scarichiamo "molti secondi di video leggeri", riempiendo il serbatoio di sicurezza prima che si svuoti del tutto.

In sintesi, il controllore agisce proprio come un rubinetto inverso: per riempire il serbatoio (buffer) dobbiamo "stringere" la qualità (abbassarla), mentre per svuotarlo possiamo "aprire" la qualità al massimo.