Nel TCP tradizionale (Reno/NewReno), quando si verifica una perdita di pacchetto (3 ACK duplicati o timeout):

1. Il protocollo assume che il router sia pieno.  
2. Dimezza ciecamente la **Congestion Window** ($cwnd$) e la soglia di Slow Start ($ssthresh$).
3. Riduce drasticamente la velocità di trasmissione.

Analizziamo con più precisione i due episodi di congestione:

- **3 ACK duplicati**: 
	- $sstresh = cwnd / 2$
	- $cwnd = sshtresh$
- **Timeout**: 
	- $sstresh = cwnd / 2$
	- $cwnd = 2$

==Dimezzare ciecamente la cwnd è un opzione troppo azzardata e poco ottimizzata.==

# TCP westwood

==TCP Westwood introduce un concetto chiave: la Stima della Banda Disponibile (Bandwidth Estimation - BWE). Invece di reagire "ciecamente" alla perdita, cerca di capire quanta banda è effettivamente disponibile. Il protocollo non guarda il flusso come un continuo indistinto, ma divide il tempo in intervalli discreti.== Immaginiamo di scattare delle "fotografie" della rete a istanti precisi: $t_k$ (tempo attuale) e $t_{k+1}$ (tempo successivo). Tra questi due istanti ($t_k$ e $t_{k+1}$), il protocollo conta quanti **ACK** (conferme di ricezione) sono tornati indietro. Se tornano molti ACK, significa che i dati stanno arrivando a destinazione velocemente, invece se ne tornano pochi, la rete è lenta. Usando questi dati possiamo fare questo calcolo:
$$\text{Banda Stimata} = \frac{\text{Dati confermati (ACK)}}{\text{Tempo trascorso } (t_{k+1} - t_k)}$$

Questo dà al protocollo un'idea reale di quanto "tubo" è disponibile per far passare i dati. Le reti sono rumorose e instabili (jitter). Una singola misurazione potrebbe essere sbagliata o troppo fluttuante. Per questo motivo, si utilizza un **"first order filter"** (un filtro del primo ordine, solitamente un filtro passa-basso) che prende la stima appena calcolata e la media con le stime passate e ottiene una curva "liscia" e stabile della larghezza di banda, ignorando picchi improvvisi o cali momentanei.

> [!NOTE] Problema del TCP westwood
> Il TCP westwood soffriva di un fenomeno chiamato **ACK Compression**. Immaginiamo che la rete sia lenta nel senso inverso (dal server a noi). I router potrebbero trattenere un gruppo di ACK e poi spararceli tutti insieme in un colpo solo (una raffica). Il Westwood originale, vedendo arrivare 5 ACK in un millisecondo, pensava: _"Wow! 5 pacchetti in 1ms? La rete è velocissima!"_. Sovrastimava la banda (vedeva picchi falsi) e non riduceva abbastanza la finestra, peggiorando la congestione. Qui l'effetto di "aliasing" (campionare il rumore invece del segnale) era fortissimo.

# TCP Westwood+ (La correzione)

==Questa versione è più calma con un approccio anti-aliasing. Invece di guardare ogni singolo ACK, aspetta un intero RTT.==

$$\text{Banda Stimata} = \frac{\text{acked\_last\_RTT}}{RTT}$$

La formula usata per il calcolo della $cwnd$ è:

$$\text{cwnd} = \text{BWE} \times \text{RTT}_{min}$$

> [!NOTE] Perché questa formula?
> - **BWE (Larghezza di Banda stimata):** È il diametro del tubo.
> - **RTT (Tempo di viaggio):** È la lunghezza del tubo.
> - **Moltiplicandoli ($BWE \times RTT$):** Ottieniamo il **volume** del tubo.

Analizzando gli episodi di congestione nel TCP Westwood, osserviamo un notevole miglioramento rispetto al TCP Reno:

- **3 ACK duplicati**: 
	- $sstresh = \hat{bw}_k RTT_{min}$
	- $cwnd = sshtresh$
- **Timeout**: 
	- $sstresh = \hat{bw}_k RTT_{min}$
	- $cwnd = 2$