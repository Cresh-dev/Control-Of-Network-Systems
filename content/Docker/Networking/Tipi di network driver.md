
_Tag:_ #docker #rete

---
Docker offre diversi driver di rete. I principali sono:

|Tipo di driver|Descrizione|
|---|---|
|`bridge`|Default per container standalone, crea una rete virtuale locale sul host|
|`host`|Usa direttamente la rete dell’host (non c’è isolamento)|
|`none`|Nessuna connessione di rete (completamente isolato)|
|`overlay`|Per la comunicazione tra container su più host (con Docker Swarm)|
|`macvlan`|Il container si comporta come un dispositivo fisico nella rete dell’host|
|`ipvlan`|Variante avanzata di `macvlan`, usata in casi specifici|

## bridge (Predefinita)

È il driver predefinito. Docker crea una rete chiamata `bridge` all'avvio. I container collegati possono comunicare tra loro tramite IP o nome (se definiti in DNS interno).

```bash
docker network ls
docker network inspect bridge
```