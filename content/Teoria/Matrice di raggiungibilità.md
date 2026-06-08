_Tag:_ #automatica #cns

---
==La matrice di raggiungibilità è uno strumento fondamentale nell'automatica per determinare se è possibile guidare un sistema dinamico da uno stato iniziale a un qualsiasi stato finale desiderato in un tempo finito, agendo esclusivamente sugli ingressi==. Dove $x$ è il vettore di stato di dimensione $n$, la raggiungibilità dipende esclusivamente dalle matrici $A$ (matrice dinamica) e $B$ (matrice degli ingressi). Per un sistema con $n$ stati, la matrice di raggiungibilità è definita come la composizione a blocchi delle potenze della matrice $A$ moltiplicate per $B$:

$$R = [B \quad AB \quad A^2B \quad \dots \quad A^{n-1}B]$$

Se lo stato ha dimensione $n$ e l'ingresso ha dimensione $m$, la matrice $R$ avrà dimensioni $n \times (n \cdot m)$. ==Un sistema si dice completamente raggiungibile se e solo se la matrice R ha rango pieno==, ovvero:
$$
\text{rank}(R) = n
$$
Se il rango è inferiore a $n$, il sistema presenta una parte non raggiungibile: esistono cioè degli stati che non possono essere influenzati dall'ingresso $u(t)$, indipendentemente da quanto sforzo o tempo si applichi.

> [!NOTE] Perché ci fermiamo a $n-1$
> Potrebbe sembrare strano fermarsi alla potenza $A^{n-1}B$. Questo deriva dal **Teorema di Cayley-Hamilton**, il quale afferma che ogni matrice quadrata soddisfa il proprio polinomio caratteristico. Di conseguenza, le potenze di $A$ superiori o uguali a $n$ (come $A^n, A^{n+1}, \dots$) sono combinazioni lineari delle potenze precedenti ($I, A, \dots, A^{n-1}$). Aggiungere ulteriori blocchi alla matrice non ne aumenterebbe quindi il rango.
