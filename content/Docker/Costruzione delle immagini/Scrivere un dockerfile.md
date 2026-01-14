
_Tag:_ #docker #dockerfile 

---
Un dockerfile è un documento testuale che è usato per creare un [[Container|container]]. Fornisce istruzioni per costruire l'immagine con comandi di avvio, files da copiare e altro ... Un esempio di un docker file può essere il seguente:

```docker
FROM python:3.12
WORKDIR /usr/local/app

# Install the application dependencies
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

# Copy in the source code
COPY src ./src
EXPOSE 5000

# Setup an app user so the container doesn't run as the root user
RUN useradd app
USER app

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8080"]
```

Per costruire e avviare l'immagine:

```bash
docker build -t mia-app . 
docker run -p 5000:5000 mia-app
```