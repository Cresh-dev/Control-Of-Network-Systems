
_Tag:_ #container 

---

I namespaces di Linux sono una tecnologia fondamentale per l'isolamento dei container (come Docker o Kubernetes). Sono utilizzati per creare ambienti isolati che non interferiscono tra loro. I principali tipi di namespaces nei container sono:

- **PID namespace**: isola i processi, impedendo che un container veda quelli di un altro.
- **Network namespace**: ogni container può avere la propria rete con interfacce virtuali separate.
- **Mount namespace**: permette di gestire sistemi di file separati tra container.
- **UTS namespace**: isola il nome host e il dominio di rete.
- **IPC namespace**: isola la comunicazione interprocesso tra i container.
- **User namespace**: permette di eseguire processi con privilegi differenti rispetto all'host.