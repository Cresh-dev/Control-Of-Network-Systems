
_Tag:_ #docker #images 

---
Le [[Immagini Docker|immagini]] sono costruite a partire da un dockerfile. Il più basico comando di costruzione potrebbe essere il seguente: `docker build .`. Quando si esegue una build, il builder estrae l'immagine di base, se necessario, e quindi esegue le istruzioni specificate nel Dockerfile. Come output del building otteniamo: `docker run sha256:9924dfd9350407b3df01d1a0e1033b1e543523ce7d5d5e2c83a724480ebe8f00`. Come vediamo il nome non è memorizzabile quindi utilizziamo il tag per dare ad un immagine un nome a nostro piacimento. 

## Tagging images

Il tagging delle immagini è il metodo per fornire un'immagine con un nome memorabile. Tuttavia, esiste una struttura per il nome di un'immagine. Un nome completo dell'immagine ha la seguente struttura: `[HOST[:PORT_NUMBER]/]PATH[:TAG]`

## Pubblicazione di un immagine

Una volta creata e taggata un'immagine, siamo pronti a pusharla in un [[Registri Docker|registro]]. Per farlo, usiamo il comando docker push: `docker push my-username/my-image`