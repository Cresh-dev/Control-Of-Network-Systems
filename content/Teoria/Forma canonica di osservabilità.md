_Tag:_ #automatica #cns

---
Questa forma è particolarmente utile perché permette di verificare [[Matrice di osservabilità|l'osservabilità del sistema]] e facilita la progettazione di **osservatori dello stato** (come il filtro di Kalman o l'osservatore di Luenberger).

Consideriamo un sistema descritto dalla funzione di trasferimento:

$$
G(s) = \frac{b_{n-1}s^{n-1} + \dots + b_1s + b_0}{s^n + a_{n-1}s^{n-1} + \dots + a_1s + a_0}
$$

La matrice $A$ presenta i coefficienti del denominatore della funzione di trasferimento (cambiati di segno) nell'ultima colonna, mentre la "diagonal superiore" è composta da 1. 

$$
A = \begin{bmatrix} 0 & 0 & \dots & 0 & -a_0 \\ 1 & 0 & \dots & 0 & -a_1 \\ 0 & 1 & \dots & 0 & -a_2 \\ \vdots & \vdots & \ddots & \vdots & \vdots \\ 0 & 0 & \dots & 1 & -a_{n-1} \end{bmatrix}
$$

I coefficienti del numeratore della funzione di trasferimento compongono direttamente la matrice $B$.

$$
B = \begin{bmatrix} b_0 \\ b_1 \\ b_2 \\ \vdots \\ b_{n-1} \end{bmatrix}
$$

La matrice $C$ seleziona l'ultimo elemento dello stato, mentre $D$ è solitamente nulla se il sistema è strettamente proprio.

$$
C = \begin{bmatrix} 0 & 0 & \dots & 0 & 1 \end{bmatrix}, \quad D = [0]
$$

Questa è la forma duale della [[Forma canonica di controllo|forma canonica di raggiungibilità]]. Se trasponiamo le matrici della forma di osservabilità, ottieniamo la forma di raggiungibilità di un sistema correlato. Un sistema espresso in questa forma è **sempre completamente osservabile**, a patto che non vi siano cancellazioni polo-zero nella funzione di trasferimento originale. ==Questa forma è "comoda" perché permette di imporre i poli dell'osservatore semplicemente scegliendo i guadagni che modificano l'ultima colonna della matrice A - LC==.

---
## Schema a blocchi

![[OBS.svg]]