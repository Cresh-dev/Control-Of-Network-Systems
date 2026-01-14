
_Tag:_ #container 

---

Il **Union Mount File System** è una tecnica utilizzata per unire più filesystem in un'unica vista coerente, permettendo di sovrapporre directory e file da diversi sorgenti. Questo approccio è molto usato nei container (Docker, Kubernetes) e nei sistemi live (come le distribuzioni Linux avviate da USB o CD). Un **Union Filesystem** combina più filesystem in un unico spazio, consentendo di:

- **Sovrapporre più livelli di filesystem** (es. uno in sola lettura e uno scrivibile).
- **Evitare la duplicazione di dati**: i file vengono condivisi tra più ambienti.
- **Fornire un filesystem dinamico** in cui le modifiche non alterano i dati originali.

## Come Funziona?

Un Union Filesystem utilizza **strati (layers)**:

1. **Lower Layer** – Il livello inferiore, di solito di sola lettura.
2. **Upper Layer** – Il livello superiore, che può essere modificato.
3. **Overlay (Merged View)** – L'unione dei due, presentata come un unico filesystem.