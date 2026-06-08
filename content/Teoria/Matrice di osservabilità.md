_Tag:_ #automatica #cns

---
La **matrice di osservabilità** è il concetto duale della [[Matrice di raggiungibilità|raggiungibilità]]. ==Mentre la raggiungibilità riguarda la capacità dell'ingresso di influenzare lo stato, l'osservabilità riguarda la capacità di ricostruire lo stato interno del sistema guardando solo le uscite (y) e gli ingressi (u)==. Per un sistema con $n$ variabili di stato, la matrice di osservabilità è costruita impilando verticalmente la matrice di uscita $C$ moltiplicata per le potenze della matrice dinamica $A$:

$$
O = \begin{bmatrix} C \\ CA \\ CA^2 \\ \vdots \\ CA^{n-1} \end{bmatrix}
$$

Se lo stato ha dimensione $n$ e l'uscita ha dimensione $p$, la matrice $O$ avrà dimensioni $(n \cdot p) \times n$. ==Un sistema si dice completamente osservabile se e solo se la matrice O ha rango massimo== (pari al numero di stati):
$$
\text{rank}(O) = n
$$
Se il rango è inferiore a $n$, esiste un **sottospazio di non osservabilità**. In pratica, ci sono alcune combinazioni degli stati interni che non producono alcun effetto sull'uscita: sono "invisibili" all'esterno.