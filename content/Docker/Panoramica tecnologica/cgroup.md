
_Tag:_ #container 

---

Un cgroup è un meccanismo del kernel Linux che consente di limitare, monitorare e isolare l'uso delle risorse di sistema tra diversi gruppi di processi. Viene utilizzato dai container (Docker, Kubernetes, LXC, ecc.) per garantire che un singolo container non monopolizzi le risorse della macchina.

## Principali funzionalità dei cgroups

Con i cgroups puoi:

1. **Limitare l’uso delle risorse** → Impostare limiti di CPU, memoria, disco, ecc.
2. **Dare priorità ai processi** → Controllare la quantità di CPU assegnata a ogni container.
3. **Monitorare le risorse** → Verificare il consumo di memoria o CPU in tempo reale.
4. **Isolare i processi** → Evitare che un processo influenzi altri processi in esecuzione.