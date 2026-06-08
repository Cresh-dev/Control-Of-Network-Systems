_Tag:_ #docker #rete

---
I container sono isolati tra reti diverse. Se sono su reti diverse, non comunicano a meno che non siano connessi a una rete comune.

- Possiamo collegare un container a più reti: `docker network connect rete2 container1`
- Per esporre porte verso l’esterno: `docker run -d -p 8080:80 nginx`
- Verifica connessioni: `docker network inspect rete_test`