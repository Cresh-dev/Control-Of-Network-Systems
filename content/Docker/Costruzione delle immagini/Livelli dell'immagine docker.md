_Tag:_ #docker #images 

---
- Il primo livello aggiunge comandi di base e un gestore di pacchetti, come apt.
- Il secondo livello installa un runtime Python e pip per la gestione delle dipendenze.
- Il terzo livello copia il file requirements.txt specifico di un'applicazione.
- Il quarto livello installa le dipendenze specifiche di quell'applicazione.
- Il quinto livello copia il codice sorgente effettivo dell'applicazione.

![[Pasted image 20250322140011.png]]

## Sovrapposizione degli strati

1. Dopo aver scaricato ogni livello, esso è estratto nella sua directory sul filesystem dell'host;
2. Quando avviamo un [[Container|container]] da un immagine, viene creato un file system di unione in cui i livelli vengono impilati uno sopra l'altro, creando una vista nuova e unificata.
3. Quando il container viene avviato, la sua directory radice è impostata sulla posizione di questa directory unificata, utilizzando chroot.