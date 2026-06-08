_Tag:_ #automatica #cns

---
QUIC è un protocollo che si basa su **UDP** (User Datagram Protocol) a livello di implementazione a differenza del TCP (implementato nel kernel), il controllo di congestione in QUIC/UDP avviene a **livello applicativo**. ==Il TCP classico aspetta di perdere un pacchetto per rallentare. Quic effettua un controllo migliore, perché monitora il ritardo, se esso aumenta significa che la coda si sta riempiendo e quindi bisogna rallentare prima di perdere dei pacchetti (buffer overflow).==

Definiamo i seguenti protagonisti:
- **S:** Sender (Mittente)
- **R:** Receiver (Ricevitore)
- **$t_i$:** Istante di tempo del pacchetto $i$.
- **OWD (One Way Delay):** Ritardo a senso unico ($T_{fw}$ - time forward).

La metrica chiave è l'**OWDV** (One Way Delay Variation), calcolata come la differenza tra il ritardo del pacchetto attuale e quello precedente.

$$
OWDV = OWD_{(i+1)} - OWD_{(i)}
$$

![[Screenshot 2026-01-14 at 11.13.38.png]]

Espandendo la formula con i timestamp di invio ($S$) e ricezione ($R$):

$$
OWDV = (t_{i+1}^R - t_{i+1}^S) - (t_{i}^R - t_{i}^S)
$$

Raggruppando i termini per capire cosa accade fisicamente:

$$
\text{OWDV} = \underbrace{(t_{i+1}^R - t_{i}^R)}_{\text{Intervallo al Ricevitore}} - \underbrace{(t_{i+1}^S - t_{i}^S)}_{\text{Intervallo al Mittente}}
$$

Stiamo confrontando l'intervallo di tempo tra l'arrivo dei pacchetti al ricevitore con l'intervallo di invio del mittente.

## Interpretazione dei Risultati

- **$= 0$ (Ritardo costante):** La rete è stabile. L'intervallo di ricezione è identico a quello di invio.
    
- **$> 0$ (Ritardo in aumento):** L'intervallo al ricevitore è _più grande_ di quello al mittente.
    
    - _Significato:_ La rete sta "dilatando" i tempi. Si stanno accumulando code nei router (il pacchetto $i+1$ ha messo più tempo del pacchetto $i$). Si sta formando una coda.
        
- **$< 0$ (Ritardo in diminuzione):** La coda si sta svuotando.

## Algoritmo di Controllo (Delay Control)

Il sistema utilizza una soglia per decidere come comportarsi.

1. **Misurazione:** Si misura costantemente il ritardo (OWD).
2. **Soglia:** Si definisce una soglia di ritardo massimo accettabile: **$OWD_{th}$** (Threshold).
3. **Confronto e Azione:**

|**Condizione**|**Stato della Rete**|**Azione**|
|---|---|---|
|**$OWD < OWD_{th}$**|La rete è libera.|Possiamo aumentare la velocità di invio.|
|**$OWD \ge OWD_{th}$**|Il "tubo" fisico è pieno, le code si riempiono troppo.|Bisogna ridurre la velocità.|

Quando $OWD \ge OWD_{th}$, si applica il seguente settaggio:

$$
cwnd = BWE \cdot RTT_{min}
$$
$$
ssthresh = cwnd
$$

- **$cwnd$:** Congestion Window (finestra di congestione).
- **$BWE$:** Bandwidth Estimate (stima della banda disponibile).
- **$RTT_{min}$:** Round Trip Time minimo osservato (che corrisponde al tempo di viaggio _senza code_).