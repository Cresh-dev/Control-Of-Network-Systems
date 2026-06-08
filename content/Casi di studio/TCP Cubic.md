_Tag:_ #automatica #cns

---
Come sappiamo il TCP Reno aumenta la velocità di invio (la _Congestion Window_ o `cwnd`) in modo lineare: aggiunge 1 pacchetto per ogni conferma (ACK) ricevuta. Questo funzionava bene negli anni '90, ma ha un problema enorme nelle reti moderne ad alta velocità e alta latenza (chiamate **LFN - Long Fat Networks**), come le fibre ottiche intercontinentali. Se perdiamo un pacchetto su una rete a 10 Gbps, TCP Reno dimezza la velocità e poi impiega _ore_ per risalire linearmente alla velocità massima. È troppo lento nel recupero. 

![[Pasted image 20260112151328.png]]

TCP Cubic risolve questo problema cambiando la matematica della crescita. Invece di una linea retta, usa una **funzione cubica** (una curva a forma di "S" allungata). L'equazione fondamentale che governa la finestra di congestione è:

$$
W(t) = C(t - K)^3 + W_{max}
$$