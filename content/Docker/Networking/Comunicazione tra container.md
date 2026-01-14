
_Tag:_ #docker #rete

---
Quando più container si trovano nella **stessa rete bridge custom**, possono comunicare tra loro **usando i nomi dei container** come hostname (DNS interno).

```bash
# Crea una rete bridge custom
docker network create --driver bridge rete_test

# Avvia due container nella stessa rete
docker run -dit --name container1 --network rete_test alpine
docker run -dit --name container2 --network rete_test alpine

# Entra nel primo container
docker exec -it container1 sh

# Dal container1 prova a pingare container2
ping container2

```