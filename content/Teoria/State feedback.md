In un sistema descritto nello [[Equazioni in forma di stato|spazio di stato]], il comportamento è definito da un set di variabili interne chiamate "stati" ($x$). ==L'idea dello state feedback è quella di calcolare l'ingresso di controllo u come una combinazione lineare di tutti gli stati del sistema==.

L'equazione fondamentale è:

$$
u = -Kx
$$

Dove:

- $x$ è il vettore degli stati (es. posizione, velocità, temperatura).
- $K$ è la **matrice dei guadagni** (gain matrix), che dobbiamo progettare.
- Il segno meno indica che stiamo applicando una retroazione negativa per stabilizzare il sistema.

Se prendiamo un sistema lineare tempo-invariante (LTI):

$$
\dot{x} = Ax + Bu
$$

E sostituiamo $u = -Kx$, otteniamo il sistema a ciclo chiuso:

$$
\dot{x} = (A - BK)x
$$

Scegliendo opportunamente i valori dentro la matrice $K$, possiamo spostare gli **autovalori** della matrice $(A - BK)$ dove vogliamo nel piano complesso. Questo significa che possiamo decidere noi quanto il sistema deve essere veloce, smorzato o stabile. Dobbiamo anche dire che Lo state feedback funziona perfettamente solo se il sistema è [[Matrice di raggiungibilità|completamente controllabile]].