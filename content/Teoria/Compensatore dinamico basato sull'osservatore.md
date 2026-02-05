==In ambito di teoria del controllo, il Compensatore Dinamico basato sull'osservatore è la soluzione standard quando vogliamo controllare un sistema ma non possiamo misurarne tutti gli stati interni (cosa che accade quasi sempre nella realtà)==. Possiamo unire le nozioni viste in precedenza del [[State feedback|controllore]] e dell'[[Osservazione dello stato|osservatore]] per realizzare il compensatore dinamico che stima lo stato del sistema, e usiamo quella stima per decidere l'azione di controllo. Per descrivere il sistema in forma di stato del **compensatore dinamico**, dobbiamo unire le equazioni dell'osservatore e la legge di controllo. Il compensatore è esso stesso un sistema dinamico che ha come **ingressi** le misure del sistema reale ($y$) e come **uscita** l'azione di controllo ($u$).
# Le equazioni di partenza

Ricordiamo le due componenti che abbiamo discusso:

1. **L'osservatore di Luenberger:** $\dot{\hat{x}} = A\hat{x} + Bu + L(y - C\hat{x})$
2. **La legge di controllo (retroazione della stima):** $u = -K\hat{x}$

# Derivazione della forma di stato del compensatore

Sostituiamo la legge di controllo ($u = -K\hat{x}$) nell'equazione della dinamica dell'osservatore per eliminare la variabile $u$:

$$
\dot{\hat{x}} = A\hat{x} + B(-K\hat{x}) + Ly - LC\hat{x}
$$

Ora raggruppiamo i termini che dipendono dalla stima dello stato $\hat{x}$:

$$
\dot{\hat{x}} = (A - BK - LC)\hat{x} + Ly
$$

# La forma di stato finale

Il compensatore dinamico può essere visto come un sistema a sé stante dove lo "stato interno" del controllore è proprio $\hat{x}$. Le sue equazioni sono:

$$
\begin{cases} \dot{\hat{x}} = \underbrace{(A - BK - LC)}_{A_{comp}} \hat{x} + \underbrace{L}_{B_{comp}} y \\ u = \underbrace{-K}_{C_{comp}} \hat{x} \end{cases}
$$

Dove:

- **Stato del compensatore:** $\hat{x}$
- **Ingresso del compensatore:** $y$ (l'uscita del processo reale)
- **Uscita del compensatore:** $u$ (l'azione di controllo da inviare al processo)

---
## Schema a blocchi

![[Compensatore dinamico.svg]]