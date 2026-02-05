Per capire il PageRank, dobbiamo immaginare internet come un enorme grafo (una rete):
- **Nodi (pallini):** Rappresentano le pagine Web ($P_1, P_2, \dots$).
- **Archi (frecce):** Rappresentano i Link ipertestuali da una pagina all'altra.

# La logica del "Voto"

L'idea rivoluzionaria è che **un link verso una pagina conta come un voto di fiducia**. Tuttavia, non tutti i voti hanno lo stesso peso. ==L'importanza di una pagina P è la somma dell'importanza delle pagine che la linkano, divisa per il numero di link che queste pagine fanno verso l'esterno.== 

**Esempio**: Se una pagina **importante**  ci linka, il nostro rango sale molto. Se una pagina ci linka ma linka anche altre 99 persone, il valore del suo "voto" si **diluisce** (diventa 1/100).

## La Formula Matematica

L'importanza (Rank) di una pagina $P$ si calcola così:

$$
r(P) = \sum_{Q \in B_P} \frac{r(Q)}{|Q|}
$$

**Legenda:**

- **$r(P)$**: È il Rank della pagina che stiamo analizzando.
- **$Q \in B_P$**: Sono le pagine "Backlink", ovvero quelle che puntano _verso_ $P$.
- **$r(Q)$**: È l'importanza della pagina $Q$ che ci sta linkando (più è alta, meglio è per noi).
- **$|Q|$**: È il numero totale di link in uscita da $Q$. Più link fa $Q$, meno valore passa a noi.

## Il Calcolo Iterativo (Dinamica nel tempo)

C'è un problema: per calcolare il nostro Rank serve il quello dell'altra pagina, ma per calcolare il suo serve il nostro. Come si risolve? Con un processo iterativo nel tempo.

- **Tempo $k-1$ (Ieri):** Usiamo i valori vecchi.
- **Tempo $k$ (Oggi):** Calcoliamo i nuovi valori basandoci su quelli di ieri.

Si parte assegnando a tutti lo stesso valore iniziale. Si ripete il calcolo tante volte finché i valori smettono di cambiare. Questo stato finale si chiama **Stato Stazionario** ($\pi_\infty$).

In forma matriciale si scrive:

$$
\pi_k = A \cdot \pi_{k-1}
$$

(Il vettore dei rank di oggi è uguale alla Matrice $A$ moltiplicata per il vettore dei rank di ieri).

# Esempio Pratico

Immaginiamo un piccolo web con sole **4 pagine**.

- **1** → linka solo a **2**.
- **2** → linka a **1** e **3**.
- **3** → linka solo a **4**.
- **4** → linka a **1** e **2**.

Costruiamo la matrice dove le **colonne ($j$)** sono le pagine di partenza e le **righe ($i$)** le pagine di arrivo.

$$
A = \begin{bmatrix} 0 & 1/2 & 0 & 1/2 \\ 1 & 0 & 0 & 1/2 \\ 0 & 1/2 & 0 & 0 \\ 0 & 0 & 1 & 0 \end{bmatrix}
$$

**Come leggere la matrice (Esempi):**

- **Colonna 1 (Pagina 1):** Ha un solo link in uscita verso la 2. Quindi metto **1** nella riga 2. Le altre celle sono 0.
- **Colonna 2 (Pagina 2):** Ha due link in uscita (verso 1 e 3). Quindi divide il suo voto: **1/2** alla riga 1 e **1/2** alla riga 3.

Applicando la formula, l'importanza della pagina 1 al tempo $k$ è:

$$
\pi_1(k) = \frac{1}{2}\pi_2(k-1) + \frac{1}{2}\pi_4(k-1)
$$