==Il nostro obiettivo tramite questo controllo è modellare il buffer dei chunk video, quindi noi siamo interessati effettivamente ai secondi di video immagazzinati nel buffer.== 

![[Screenshot 2026-01-17 at 09.10.13.png]]

Possiamo esprimere di seguito la variazione del buffer tramite la derivata $\dot{t_f(t)}$ che cresce se la rete ($r$) è più veloce del consumo video ($l$):

$$
\dot{t_f(t)} = \frac{r(t)}{l(t)} - o(t)
$$

Dove:

- $r(t)$ è a velocità a cui i dati video arrivano fisicamente dal server al buffer del nostro dispositivo:$\frac{\Delta Data}{\Delta t}$;
- $l(t)$ è il livello dello stream;
- $o(t)$ è la velocità di riproduzione che di regola è un secondo di film su un secondo di tempo.

$r(t)$ si comporta come un rumore nel nostro sistema e il modello non è lineare perché noi vogliamo controllare $l(t)$ che si trova al denominatore, pertanto eseguiamo la linearizzazione in retroazione (_feedback linearization_). Usando un controllore proporzionale-integrale $\dot{t_f(t)}$ diventa $-k_p t_f(t) - k_i t_{fi}(t)$, dove $t_{fi}(t)$ è l'integrale dell'errore di $t_f(t)$, mentre $k_p$ e $k_i$ sono due parametri scelti. Il modello finale che si ottiene diventa:

$$
l(t) = \frac{r(t)}{o(t) - k_p t_f(t) - k_i t_{fi}(t)}
$$

1. **Se il buffer è troppo pieno:**
    - Vogliamo che il buffer scenda (cioè che $\dot{t}_f$ diventi negativo).
    - Poiché la qualità $l(t)$ è al **denominatore**, se aumentiamo $l(t)$ (scegliendo una qualità video più alta, es. 4K), il termine $\frac{r(t)}{l(t)}$ diventa più piccolo.
    - Quando il termine di ingresso $\frac{r(t)}{l(t)}$ diventa minore della velocità di riproduzione $o(t)$ (che è fissa a 1), il buffer inizia a svuotarsi. In pratica, stai scaricando "pochi secondi di video molto pesanti", quindi consumi il buffer accumulato mentre ci godiamo l'alta qualità.
2. **Se il buffer si sta svuotando (o è basso):**
    - Vogliamo che il buffer risalga rapidamente (cioè che $\dot{t}_f$ sia positivo e grande).
    - Dobbiamo **diminuire** la qualità $l(t)$. Un valore più piccolo al denominatore fa crescere il risultato della frazione.
    - In questo modo, con la stessa velocità di rete $r(t)$, scarichiamo "molti secondi di video leggeri", riempiendo il serbatoio di sicurezza prima che si svuoti del tutto.

In sintesi, il controllore agisce proprio come un rubinetto inverso: per riempire il serbatoio (buffer) dobbiamo "stringere" la qualità (abbassarla), mentre per svuotarlo possiamo "aprire" la qualità al massimo.

# Rappresentazione in spazio di stato

Per poter usare l'algebra lineare (le matrici), raggruppiamo le variabili che cambiano nel tempo in un unico vettore colonna:

$$
x = \begin{bmatrix} t_f \\ t_{f_I} \end{bmatrix} \quad \rightarrow \text{variabili di stato}
$$

> [!NOTE] Ingresso del sistema
> Nel nostro caso l'ingresso è il set point del buffer $t_f^s$ quindi : 
> $$u = t_f^s$$

Il sistema in spazio di stato ha la seguente forma:

$$
\begin{cases} \dot{t}_f(t) = -k_P t_f(t) - k_I t_{f_I}(t) \\ \dot{t}_{f_I} = t_f^s - t_f(t) \end{cases}
$$

- La **prima equazione** ci dice che la velocità con cui cambia la variabile $t_f$ dipende dal suo valore attuale (moltiplicato per un guadagno $k_P$) e dal valore di una seconda variabile $t_{f_I}$ (moltiplicata per $k_I$).
- La **seconda equazione** descrive l'errore. La variazione della componente integrale ($\dot{t}_{f_I}$) è data dalla differenza tra il valore desiderato (il set point $t_f^s$) e il valore attuale ($t_f$). In pratica, il sistema sta "accumulando" l'errore nel tempo.

## Matrici dello spazio di stato

$$
A = \begin{bmatrix} -k_P & -k_I \\ -1 & 0 \end{bmatrix}
$$

$$
B = \begin{bmatrix} 0 \\ 1 \end{bmatrix}
$$

$$
C = \begin{bmatrix} 1 & 0 \end{bmatrix}
$$

$$
D = 0
$$

## Pole allocation

In questo caso possiamo effettuare la **pole allocation** (o allocazione degli autovalori). Possiamo scegliere matematicamente i valori di $k_P$ e $k_I$ per imporre al sistema esattamente il comportamento dinamico che desideriamo.