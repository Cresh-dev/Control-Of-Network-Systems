_Tag:_ #automatica #cns

---
Iniziamo con il definire cosa sono i sistemi caotici (come l'Attrattore di Lorenz):

- **Non lineari:** Non seguono una semplice proporzionalità diretta.
- **Deterministici:** Seguono equazioni precise (non sono casuali).
- **Sensibili alle condizioni iniziali:** Una variazione infinitesimale all'inizio porta a risultati completamente diversi dopo poco tempo (il famoso "effetto farfalla").

![[Pasted image 20260113151327.png]]

> [!NOTE] Perché usarli in crittografia?
> Poiché è impossibile conoscere le condizioni iniziali esatte del trasmettitore, il segnale generato appare come "rumore" imprevedibile a chiunque lo intercetti. Tuttavia, se conosciamo il sistema, non è vero rumore, ma caos strutturato.

## Il Problema della Sincronizzazione

Se abbiamo due sistemi caotici identici, uno che trasmette (Master, $x(t)$) e uno che riceve (Slave, $z(t)$), normalmente divergeranno rapidamente.

L'obiettivo è forzare il ricevitore ($z$) a seguire esattamente il trasmettitore ($x$) in modo che l'errore tra i due ($e(t)$) vada a zero.

$$
e(t) = x(t) - z(t) \rightarrow 0
$$

Per fare ciò, inviamo un segnale di sincronizzazione $s(x)$ dal Master allo Slave:

$$
s(x) = f(x) + Kx
$$

### Il Trasmettitore (Master)

Il sistema originale è descritto da:

$$
\dot{x}(t) = Ax(t) + Bf(x)
$$

- $Ax(t)$: Parte lineare.
- $Bf(x)$: Parte non lineare (la fonte del caos).

### Il Ricevitore (Slave) con Correzione

Il ricevitore ha la stessa struttura, ma viene aggiunto un termine di correzione derivato dal segnale $s(x)$ ricevuto:

$$
\dot{z}(t) = Az(t) + Bf(z) + \underbrace{B(f(x)+Kx)−B(f(z)+Kz)}_{Termini \ di \ correzione}
$$

L'obiettivo della correzione è cancellare la differenza tra i due sistemi.

### La Dinamica dell'Errore

Sottraendo l'equazione del ricevitore da quella del trasmettitore, otteniamo la derivata dell'errore $\dot{e}(t)$:

$$
\dot{e}(t) = A(z(t)-x(t)) + BKx + BKz = Ae -BKe = (A-BK)e
$$

Questa è la classica equazione di un **Osservatore di Stato**. Se scegliamo la matrice $K$ in modo corretto, possiamo spostare gli **autovalori** (eigenvalues) della matrice $(A - BK)$ nel semipiano negativo reale ($\mathbb{R}^-$). L'errore $e(t)$ decade esponenzialmente a 0. I due sistemi sono ora sincronizzati ($x = z$).


> [!NOTE] Osservazione
> Il **ruolo funzionale** del sistema Slave è quello di un **Osservatore**. Il suo scopo non è stabilizzare il Master (che deve rimanere caotico/instabile!), ma solo **ricostruirne (osservarne) lo stato** $x(t)$ partendo da un'informazione parziale $s(x)$.

