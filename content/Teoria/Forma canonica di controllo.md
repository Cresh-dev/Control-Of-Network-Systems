_Tag:_ #automatica #cns

---
Usiamo questa rappresentazione quando vogliamo imporre al sistema un comportamento desiderato tramite il **ritorno di stato** ($u = -Kx$).

Dato un sistema descritto dalla funzione di trasferimento:

$$
G(s) = \frac{b_{n-1}s^{n-1} + \dots + b_1s + b_0}{s^n + a_{n-1}s^{n-1} + \dots + a_1s + a_0}
$$

Nella forma canonica di controllo, le matrici $A$, $B$, $C$ assumono una struttura "fissa" basata sui coefficienti del denominatore e del numeratore:

**Matrice di stato $A$:** Presenta una riga (solitamente l'ultima) contenente i coefficienti del polinomio caratteristico cambiati di segno, e una "sopradiagonale" di 1.

$$
A = \begin{bmatrix} 0 & 1 & 0 & \dots & 0 \\ 0 & 0 & 1 & \dots & 0 \\ \vdots & \vdots & \vdots & \ddots & \vdots \\ 0 & 0 & 0 & \dots & 1 \\ -a_0 & -a_1 & -a_2 & \dots & -a_{n-1} \end{bmatrix}
$$
 **Matrice di ingresso $B$:** È un vettore colonna nullo tranne che nell'ultimo elemento.
 
$$
B = \begin{bmatrix} 0 \\ 0 \\ \vdots \\ 0 \\ 1 \end{bmatrix}
$$
**Matrice di uscita $C$:** Contiene i coefficienti del numeratore.
   
$$
C = \begin{bmatrix} b_0 & b_1 & \dots & b_{n-1} \end{bmatrix}
$$

Un sistema in questa forma è, per definizione, **completamente raggiungibile**. La [[Matrice di raggiungibilità|matrice di raggiungibilità]] ha rango massimo. I coefficienti dell'ultima riga di $A$ sono esattamente i coefficienti del polinomio caratteristico $P(s) = \det(sI - A)$. Questo rende banale il calcolo degli autovalori. ==Se applichiamo una retroazione dello stato, cambiare i poli del sistema equivale semplicemente a sommare i valori di K agli elementi dell'ultima riga di A==.

---
## Schema a blocchi

![[CNTR.svg]]