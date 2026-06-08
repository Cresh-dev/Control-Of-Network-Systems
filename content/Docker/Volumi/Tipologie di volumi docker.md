_Tag:_ #docker #volumi

---
- **Volumi nominati**: creati con `docker volume create nome_volume`, riutilizzabili da più container;
- **Volumi anonimi**: creati automaticamente se si monta un percorso interno senza nome, sono difficili da gestire manualmente;
- **Bind mounts**: collegano una cartella esistente dell’host al container tramite percorso assoluto (e.g. `/home/...:/data`);
- **tmpfs mounts**: volatili, memorizzano dati in RAM e vengono persi allo stop del container.