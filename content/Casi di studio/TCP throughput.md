_Tag:_ #automatica #cns

---
Il **TCP Throughput** (o _throughput effettivo_) è una delle metriche più importanti per capire le reali prestazioni di una rete, questo valore indica la **quantità effettiva di dati che viene trasmessa con successo** (in byte o bit) in una data unità di tempo su una connessione TCP.

## Calcolo della formula di Mathis

![[Screenshot 2026-01-09 at 15.46.25.png]]

Dall'immagine possiamo calcolare l'area $A$ che è la quantità di pacchetti che possiamo spedire in un ciclo completo (tra una perdita e l'altra). Eseguiamo quindi i seguenti passaggi per arrivare alla formula finale:
1. Calcoliamo l'area del rettangolo che ha altezza $\frac{w}{2}$ e base $w-\frac{w}{2}=\frac{w}{2}$. L'area del rettangolo è il prodotto $\frac{w}{2} \cdot \frac{w}{2} = \frac{w^2}{4}$. 
2. Calcoliamo l'area del triangolo rettangolo che ha altezza $\frac{w}{2}$ e base $w-\frac{w}{2}=\frac{w}{2}$. L'area del triangolo è il prodotto $\frac{1}{2} \cdot \frac{w}{2} \cdot \frac{w}{2} = \frac{w^2}{8}$. 
3. Sommiamo le due aree: $\frac{w^2}{4} + \frac{w^2}{8} = \frac{3w^2}{8}$.
4. Uguagliamo l'area al numero di pacchetti in un ciclo (inverso della probabilità di perdita) $\frac{3w^2}{8} = \frac{1}{P}$ e isoliamo la finestra di congestione ($w$)  per trovare la sua dimensione $w = \sqrt{\frac{8}{3}} \cdot \frac{1}{\sqrt P}$.
5. Dividiamo il tutto per RTT $\frac{w}{RTT} = \sqrt{\frac{8}{3}} \cdot \frac{1}{\sqrt P}\cdot \frac{1}{RTT} = \frac{k}{\sqrt{P} \cdot RTT}$ e otteniamo i throughput ($T$) cioè i dati ($w$) sopra l'unità di tempo ($RTT$): $T = \frac{k}{\sqrt{P} \cdot RTT}$

> [!NOTE] Attenzione alla probabilità di perdita
> Dalla formula di mathis possiamo osservare che per avere delle velocità molto alte, $P$ deve essere un numero piccolissimo perché altrimenti la radice quadrata al denominatore "ucciderebbe" il throughput. Per questo esatto motivo abbiamo bisogno di modificare il protocollo TCP e variare il suo comportamento proprio come fa il [[TCP Cubic]].