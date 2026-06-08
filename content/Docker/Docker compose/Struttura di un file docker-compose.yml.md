_Tag:_ #docker #docker-compose

---
Immaginiamo di voler sviluppare un’applicazione composta da:

- Un'app backend in Node.js;
- Un database PostgreSQL;
- Un servizio di caching Redis.

Con Docker Compose possiamo descrivere **tutti questi servizi** e le loro configurazioni **in un unico file**, ed eseguire tutto con un solo comando: `docker-compose up`.

Ecco un esempio base per un'applicazione web che usa **Nginx** e **PHP-FPM**:

```yaml
version: '3.8'  # Specifica la versione del formato Compose

services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
      - ./app:/var/www/html
    depends_on:
      - php

  php:
    image: php:8.2-fpm
    volumes:
      - ./app:/var/www/html
```

## Spiegazione delle direttive principali

|Direttiva|Significato|
|---|---|
|`version`|Versione dello schema del file YAML.|
|`services`|Lista dei servizi/container da gestire.|
|`image`|L’immagine Docker da usare per quel servizio.|
|`ports`|Mapping delle porte host:container.|
|`volumes`|Condivisione di file tra host e container.|
|`depends_on`|Indica che un servizio dipende da un altro (es. `web` dipende da `php`).|

## Comandi principali di Docker Compose

- Avvio: `docker-compose up`;
- Stop e rimozione dei [[Container|container]]: `docker-compose down`
- Ricostruzione (utile dopo una modifica a Dockerfile): `docker-compose up --build`
- Mostrare i container in esecuzione: `docker-compose ps`

## Best Practices

- Usare **`.env`** per gestire variabili d’ambiente.
- Isola le configurazioni per ambiente (es. `docker-compose.prod.yml`).
- Mantenere i servizi modulari e semplici.
- Usare **volumi persistenti** per i dati (es. database).