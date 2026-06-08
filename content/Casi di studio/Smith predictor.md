_Tag:_ #automatica #cns

---
==Lo Smith Predictor serve a migliorare il controllo di sistemi con ritardo puro (dead time), cioè sistemi in cui tra l’azione di controllo e l’effetto sull’uscita passa un tempo significativo. Il problema principale è che il ritardo degrada fortemente le prestazioni di un controllore classico (come PID): può rendere il sistema lento, oscillante o addirittura instabile==. Lo Smith Predictor nasce proprio per **“aggirare” il ritardo** durante il progetto del controllo. Esso si ottiene uguagliando la funzione di trasferimento (FdT) dei due sistemi: il sistema iniziale e il sistema in cui il ritardo viene portato fuori dall'anello di retroazione. Dopo aver imposto l'uguaglianza, si otterrà una funzione di trasferimento del controllore che sarà equivalente alla FdT iniziale, ma senza considerare il ritardo. 

![[Screenshot 2026-01-17 at 09.04.13.png]]

Questo controllore è chiamato predittore di Smith:

$$
G_c(s) = \frac{K(s)}{1 + K(s)G_p(s)(1 - e^{-sT})}
$$

Per chiarezza, ecco cosa rappresentano i termini nell'equazione:

- **$G_c(s)$**: È il **Predittore di Smith** completo (il controllore modificato).
- **$K(s)$**: È il controllore primario (spesso indicato anche come $C(s)$ o $R(s)$) progettato per il processo _senza_ ritardo.
- **$G_p(s)$**: È il modello del processo (impianto) **senza** il ritardo.
- **$e^{-sT}$**: Rappresenta il ritardo di tempo puro (tempo morto) $T$.
- **$1 - e^{-sT}$**: È la parte che "predice" l'errore tra il modello senza ritardo e il modello con ritardo.