La linearizzazione è un passaggio fondamentale nello studio dei sistemi dinamici, essa ci ==permette di "approssimare" il comportamento di un sistema complesso e non lineare con uno lineare (molto più semplice da studiare) nelle immediate vicinanze di un punto di equilibrio==. Partiamo da un sistema dinamico espresso da un'equazione differenziale vettoriale non lineare:

$$
\dot{x} = f(x, u)
$$
Dove:

- $x$ è lo stato del sistema.
- $u$ è l'ingresso.
- $f$ è una funzione non lineare.

Un punto di equilibrio $(x_0, u_0)$ è una condizione in cui il sistema, se non disturbato, rimane fermo. Matematicamente:

$$
f(x_0, u_0) = 0
$$

Per linearizzare, ci spostiamo di una piccola quantità $\delta x$ e $\delta u$ dal punto di equilibrio:

- $x = x_0 + \delta x$
- $u = u_0 + \delta u$

Espandiamo la funzione $f(x, u)$ in serie di Taylor attorno a $(x_0, u_0)$, fermandoci al **primo ordine** (trascurando i termini di ordine superiore):

$$
\dot{x} \approx f(x_0, u_0) + \left. \frac{\partial f}{\partial x} \right|_{(x_0, u_0)} \delta x + \left. \frac{\partial f}{\partial u} \right|_{(x_0, u_0)} \delta u
$$

Siccome $f(x_0, u_0) = 0$ (per definizione di equilibrio) e $\dot{x} = \dot{x}_0 + \dot{\delta x} = \dot{\delta x}$, otteniamo il sistema linearizzato. Il sistema finale assume la classica forma lineare tempo-invariante (LTI):

$$
\dot{\delta x} = A \delta x + B \delta u
$$

Le matrici $A$ e $B$ sono chiamate **Matrici Jacobiane** e contengono le derivate parziali calcolate nel punto di equilibrio:

Descrive la dinamica interna: $A = \begin{bmatrix} \frac{\partial f_1}{\partial x_1} & \dots & \frac{\partial f_1}{\partial x_n} \\ \vdots & \ddots & \vdots \\ \frac{\partial f_n}{\partial x_1} & \dots & \frac{\partial f_n}{\partial x_n} \end{bmatrix}_{(x_0, u_0)}$

Descrive come l'ingresso influenza lo stato: $B = \begin{bmatrix} \frac{\partial f_1}{\partial u_1} & \dots & \frac{\partial f_1}{\partial u_m} \\ \vdots & \ddots & \vdots \\ \frac{\partial f_n}{\partial u_1} & \dots & \frac{\partial f_n}{\partial u_m} \end{bmatrix}_{(x_0, u_0)}$