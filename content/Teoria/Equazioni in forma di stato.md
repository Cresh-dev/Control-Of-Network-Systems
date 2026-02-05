Le **equazioni in forma di stato** sono uno strumento fondamentale per descrivere il comportamento di sistemi dinamici (meccanici, elettrici, economici, ecc.) nel tempo. Invece di usare una singola equazione complicata, questo metodo scompone il sistema in un set di equazioni differenziali del primo ordine. ==L'idea è quella di conoscere lo "stato" interno del sistema in ogni istante==. I componenti principali sono due equazioni matriciali:

1. **L'equazione di stato**: Descrive come cambia lo stato interno $x(t)$ in base allo stato attuale e agli ingressi $u(t)$.
2. **L'equazione di uscita**: Descrive come le uscite misurabili $y(t)$ dipendono dallo stato e dagli ingressi.

$$
\dot{x}(t) = Ax(t) + Bu(t)
$$

$$
y(t) = Cx(t) + Du(t)
$$
# Le Quattro Matrici Fondamentali 

Le matrici definiscono le relazioni "matematiche" tra queste variabili:

|**Matrice**|**Nome**|**Cosa fa?**|
|---|---|---|
|**$A$**|**Matrice di Dinamica**|Determina come il sistema evolve da solo (stabilità, oscillazioni).|
|**$B$**|**Matrice di Ingresso**|Descrive come l'ingresso $u(t)$ influenza il cambiamento dello stato.|
|**$C$**|**Matrice di Uscita**|Lega lo stato interno $x(t)$ a ciò che vediamo effettivamente in uscita.|
|**$D$**|**Matrice di Legame Diretto**|Rappresenta un effetto immediato dell'ingresso sull'uscita (spesso è zero).|
# Trasformazione di similitudine

Se sostituiamo $x = Tz$ nelle equazioni originali e facciamo un po' di passaggi algebrici, otteniamo un nuovo sistema nelle variabili $z$:

- **Nuova matrice di dinamica:** $\tilde{A} = T^{-1}AT$
- **Nuova matrice di ingresso:** $\tilde{B} = T^{-1}B$
- **Nuova matrice di uscita:** $\tilde{C} = CT$
- **Matrice di legame diretto:** $\tilde{D} = D$ (questa non cambia mai!)

Due matrici $A$ e $\tilde{A}$ legate dalla relazione $\tilde{A} = T^{-1}AT$ si dicono **simili**. Anche se le matrici sembrano diverse, alcune proprietà fondamentali del sistema **non cambiano mai**, indipendentemente dalla $T$ che scegli:

1. **Gli Autovalori:** I poli del sistema (che determinano la stabilità) restano gli stessi.
2. **La Funzione di Trasferimento:** Il rapporto ingresso/uscita non cambia.
3. **Deteriminante e Traccia** della matrice $A$.