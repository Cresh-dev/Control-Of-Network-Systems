
_Tag:_ #docker #Multi-stage 

---
In una build tradizionale, tutte le istruzioni di build vengono eseguite in sequenza e in un singolo contenitore di build: download delle dipendenze, compilazione del codice e confezionamento dell'applicazione. Tutti questi livelli finiscono nell'[[Immagini Docker|immagine]] finale. Questo approccio funziona, ma porta a immagini ingombranti che comportano un peso non necessario e aumentano i rischi per la sicurezza. È qui che entrano in gioco le build multi-fase. Le build multi-fase introducono più fasi nel tuo Dockerfile, ciascuna con uno scopo specifico. Pensala come la possibilità di eseguire diverse parti di una build in più ambienti diversi, contemporaneamente. Separando l'ambiente di build dall'ambiente di runtime finale, puoi ridurre significativamente le dimensioni dell'immagine e la superficie di attacco. Ciò è particolarmente utile per le applicazioni con grandi dipendenze di build. Le build multi-fase sono consigliate per tutti i tipi di applicazioni. Di seguito abbiamo un esempio:

```docker
# Stage 1: Build Environment
FROM builder-image AS build-stage 
# Install build tools (e.g., Maven, Gradle)
# Copy source code
# Build commands (e.g., compile, package)

# Stage 2: Runtime environment
FROM runtime-image AS final-stage  
#  Copy application artifacts from the build stage (e.g., JAR file)
COPY --from=build-stage /path/in/build/stage /path/to/place/in/final/stage
# Define runtime configuration (e.g., CMD, ENTRYPOINT) 
```