==Lo State Observer (osservatore dello stato) è un algoritmo (un "sensore virtuale") che stima lo stato interno di un sistema partendo solo dalle misure degli ingressi e delle uscite==. L'osservatore più comune è quello di **Luenberger**. Funziona facendo girare una simulazione del sistema in parallelo al sistema reale. Il sistema reale è descritto dalle [[Equazioni in forma di stato|equazioni di stato]]:

$$
\dot{x} = Ax + Bu
$$

$$
y = Cx
$$

L'osservatore crea una stima $\hat{x}$ usando la stessa struttura, ma aggiungendo un **termine di correzione**:

$$
\dot{\hat{x}} = A\hat{x} + Bu + L(y - \hat{y})
$$

Dove:

- **$A, B, C$**: Sono le matrici che descrivono la dinamica del sistema.
- **$u$**: È l'ingresso (comando) che diamo al sistema.
- **$y$**: È l'uscita misurata dal sensore reale.
- **$\hat{y}$**: È l'uscita che l'osservatore _pensa_ che il sistema dovrebbe avere.
- **$L$**: È il **Guadagno dell'Osservatore** (Observer Gain).

> [!NOTE] L'osservatore deve avere poli più veloci del controllore
> Il controllore (la legge di controllo $u = -K\hat{x}$) prende decisioni basandosi sulla stima $\hat{x}$ fornita dall'osservatore. Se l'**osservatore è lento**, la sua stima $\hat{x}$ impiegherà molto tempo per "inseguire" lo stato reale $x$. Il **controllore**, nel frattempo, userà un'informazione vecchia o errata per agire sul sistema.

Il termine $L(y - \hat{y})$ è fondamentale. Se l'errore $(y - \hat{y})$ è grande, l'osservatore corregge la sua stima interno per "inseguire" la realtà. Se il modello fosse perfetto e non ci fossero disturbi, l'errore di stima tenderebbe a zero nel tempo. Non tutti i sistemi permettono di usare un osservatore. Un sistema si dice [[Matrice di osservabilità|osservabile]] se è possibile ricostruire lo stato iniziale $x(0)$ guardando l'uscita $y(t)$ per un tempo finito.