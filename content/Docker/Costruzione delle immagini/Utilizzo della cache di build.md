_Tag:_ #docker #build-cache

---

Quando viene eseguito il comando `docker build` per creare una nuova [[Immagini Docker|immagine]], Docker processa ogni istruzione presente nel [[Scrivere un dockerfile|Dockerfile]], creando un layer per ciascun comando e seguendo l’ordine specificato. Per ogni istruzione, Docker verifica se è possibile riutilizzare un layer generato durante una build precedente. Se Docker rileva di aver già eseguito un’istruzione identica in passato, non è necessario ripeterla: viene invece utilizzato il risultato memorizzato nella cache.

Grazie a questo meccanismo, il processo di build risulta più veloce ed efficiente, permettendo di risparmiare tempo e risorse. Un utilizzo efficace della cache di build consente infatti di ottenere build più rapide, riutilizzando i risultati delle build precedenti ed evitando operazioni superflue. Per massimizzare i benefici della cache ed evitare ricostruzioni costose in termini di tempo e risorse, è fondamentale comprendere il funzionamento dell’invalidazione della cache.

Di seguito sono riportati alcuni esempi di situazioni che possono causare l’invalidazione della cache:

- Qualsiasi modifica al comando di un’istruzione **RUN** invalida il layer corrispondente. Docker rileva la modifica e invalida la cache di build quando il comando RUN nel Dockerfile viene alterato.
- Qualsiasi modifica ai file copiati nell’immagine tramite le istruzioni **COPY** o **ADD**. Docker monitora i cambiamenti all’interno della directory del progetto: sia le modifiche al contenuto dei file sia quelle alle loro proprietà, come i permessi, vengono considerate trigger per l’invalidazione della cache.
- Una volta che un layer viene invalidato, anche tutti i layer successivi vengono invalidati. Se un livello precedente — inclusa l’immagine di base o un livello intermedio — subisce un’invalidazione, Docker invalida automaticamente anche i livelli che dipendono da esso. Questo comportamento garantisce la coerenza del processo di build ed evita incongruenze.
- Durante la scrittura o la modifica di un Dockerfile, è importante prestare attenzione ai cache miss non necessari, così da assicurare che le build vengano eseguite nel modo più rapido ed efficiente possibile.