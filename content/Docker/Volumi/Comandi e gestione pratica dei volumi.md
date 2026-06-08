_Tag:_ #docker #volumi 

---

**Creazione di un volume**: 

```bash
docker volume create dati_db
docker run -v dati_db:/var/lib/mysql ...
```

- **Elencare**: `docker volume ls`
- **Ispezionare**: `docker volume inspect dati_db`
- **Rimuovere**: `docker volume rm nome_volume` o `docker volume prune` per eliminare quelli inutilizzati.

## In docker-compose

```yaml
services:
  db:
    image: mysql
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:
```