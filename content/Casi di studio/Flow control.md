_Tag:_ #automatica #cns

---
==Il controllo di flusso è un'operazione che consente al mittente di regolare il proprio tasso di invio per adattarsi alla capacità del buffer del ricevitore (evitando così il buffer overflow) tramite la advertised window (awnd)==. L'$awnd$ indica quanto spazio ha ancora a disposizione il buffer del ricevitore. Per modellare il controllo di flusso utilizziamo $i(t)$, $o(t)$, $q(t)$ e $r(t)$, che rappresentano rispettivamente il tasso di ingresso, il tasso di uscita, il buffer disponibile e la lunghezza del buffer. Esistono inoltre due ritardi: uno per l'inoltro (_forwarding_) e uno per il ritorno (_feedback_) dell'$awnd$ al mittente (modellati come $e^{-sRTT}$, dove $RTT$ è la somma dei due tempi). Il processo fisico è modellato come un integratore. 

![[Screenshot 2026-01-08 at 21.42.05.png]]

> [!NOTE] Problema del flow control
> Tuttavia, un controllore standard $G_c(s)$ non funzionerebbe correttamente: la componente di ritardo rende il sistema instabile (trasformando la retroazione negativa in una positiva). Per questo utilizziamo un **[[Smith predictor|Predittore di Smith]]**. La progettazione avviene in due fasi: inizialmente definiamo un controllore primario $k(s)$ calcolato sul **sistema privo di ritardo** (modello ideale) per garantirne le prestazioni desiderate. Successivamente, sfruttando la struttura del Predittore di Smith, ricaviamo il controllore effettivo $G_c(s)$ da inserire nel sistema reale, compensando così il tempo morto presente nell'anello di retroazione."
