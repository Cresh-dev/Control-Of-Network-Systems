Usiamo questa forma quando vogliamo "disaccoppiare" il sistema, ogni variabile di stato diventa indipendente dalle altre. Con questa rappresentazione rendiamo la matrice $A$ diagonal, ovvero con i valori solo sulla diagonal principale. Questi valori sono gli **autovalori** ($\lambda_1, \lambda_2, ...$) di $A$ cioè i poli del nostro sistema.

Data una generica rappresentazione dello stato:

$$
\begin{cases} \dot{x} = Ax + Bu \\ y = Cx + Du \end{cases}
$$

Se la matrice $A$ ha $n$ autovalori distinti $\lambda_1, \lambda_2, ..., \lambda_n$, esiste una trasformazione di coordinate $x = Tz$ che porta il sistema nella forma:

# La Matrice $A$

$$
A_{diag} = \Lambda = \begin{bmatrix} \lambda_1 & 0 & \cdots & 0 \\ 0 & \lambda_2 & \cdots & 0 \\ \vdots & \vdots & \ddots & \vdots \\ 0 & 0 & \cdots & \lambda_n \end{bmatrix}
$$

Ogni elemento sulla diagonal principale rappresenta un **polo** della funzione di trasferimento del sistema.

# Le Matrici $B$ e $C$

In questa forma, i vettori $B$ e $C$ assumono un significato fisico preciso:

- **$B_{diag} = [b_1, b_2, ..., b_n]^T$**: Se un elemento $b_i = 0$, l'i-esimo modo non è **eccitabile** dall'ingresso (non controllabile).
- **$C_{diag} = [c_1, c_2, ..., c_n]$**: Se un elemento $c_i = 0$, l'i-esimo modo non è **osservabile** dall'uscita.
# Esempio 

Avendo questo sistema di partenza:

$$
\dot{x} = \begin{bmatrix} 0 & 1 \\ -2 & -3 \end{bmatrix} x + \begin{bmatrix} 0 \\ 1 \end{bmatrix} u
$$

$$
y = \begin{bmatrix} 1 & 0 \end{bmatrix} x
$$
Il primo passo è trovare gli **autovalori** risolvendo $\det(\lambda I - A) = 0$:

$$
\det \begin{bmatrix} \lambda & -1 \\ 2 & \lambda + 3 \end{bmatrix} = \lambda^2 + 3\lambda + 2 = 0 \implies (\lambda+1)(\lambda+2)=0
$$

Gli autovalori sono $\lambda_1 = -1$ e $\lambda_2 = -2$. Per calcolare la matrice di trasformazione $T$ dobbiamo trovare gli **autovettori** $v_1$ e $v_2$ risolvendo $(A - \lambda_i I)v_i = 0$.

- Per $\lambda_1 = -1$, l'autovettore è $v_1 = \begin{bmatrix} 1 \\ -1 \end{bmatrix}$
- Per $\lambda_2 = -2$, l'autovettore è $v_2 = \begin{bmatrix} 1 \\ -2 \end{bmatrix}$

La matrice di trasformazione è:

$$
T = [v_1 | v_2] = \begin{bmatrix} 1 & 1 \\ -1 & -2 \end{bmatrix}
$$

Usando $\tilde{A} = T^{-1}AT$, otteniamo:

$$
\dot{z} = \begin{bmatrix} -1 & 0 \\ 0 & -2 \end{bmatrix} z + \tilde{B}u
$$