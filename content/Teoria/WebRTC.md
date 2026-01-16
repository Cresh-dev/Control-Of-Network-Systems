Per introdurre questo concetto dobbiamo partire da una situazione in cui due utenti vogliono stabilire una videoconferenza o scambiare dati (es. gaming, chat), ma spesso sorge il dubbio su quale software usare, se è compatibile con il sistema operativo o se funziona su tutti i dispositivi. ==WebRTC  è una tecnologia con l'idea di utilizzare il browser come piattaforma unificante, trasformando la voce e il video in un'altra applicazione JavaScript, senza bisogno di plugin esterni.==

# Standardizzazione e Architettura

WebRTC è frutto della collaborazione tra due enti di standardizzazione:

- **IETF:** Si occupa dei protocolli di rete (audio, video, sicurezza, attraversamento NAT).
- **W3C:** Definisce le API per i browser (JavaScript).

L'architettura descritta di seguito è il "Triangolo WebRTC":

![[Screenshot 2026-01-11 at 17.48.38.png]]

1. Due browser (Caller e Callee) che comunicano direttamente (P2P) per i media.
2. Un **Web Service** (Signaling Server) che serve solo per far "incontrare" i due utenti e scambiare le informazioni iniziali necessarie a stabilire la connessione.

# Le 3 API Principali

WebRTC funziona in pratica attraverso tre API standardizzate dal W3C:

- **MediaStream:** ==Permette l'accesso alla webcam e al microfono dell'utente. Utilizza la funzione navigator.getUserMedia che richiede esplicitamente il permesso all'utente.==
- **RTCPeerConnection:** ==È il cuore della comunicazione. Gestisce la connessione Point-to-Point (P2P), la codifica audio/video, e l'attraversamento dei NAT (le barriere di rete domestiche).==
- **RTCDataChannel:** ==Un canale per inviare dati generici (non solo audio/video) direttamente tra i browser.==

# Il Processo di Connessione (Handshake)

Scendiamo nel dettaglio tecnico di come avviene la connessione:

- **NAT Traversal:** Poiché i computer sono spesso dietro router privati (NAT), si usano protocolli come **STUN** e **ICE** per scoprire il proprio indirizzo IP pubblico.
- **SDP (Session Description Protocol):** I browser si scambiano "Offerte" e "Risposte" (SDP Offer/Answer) per negoziare i dettagli della chiamata (quali codec usare, quali protocolli, ecc.).
- **Signaling:** WebRTC non dice _come_ inviare questi messaggi (possiamo usare WebSocket, Socket.io, HTTP, etc.), ma dice _cosa_ inviare.

## Peerconnection workflow

![[Screenshot 2026-01-12 at 15.11.29.png]]

1. Entrambi i lati devono creare l'oggetto `RTCPeerConnection`. È come se entrambi alzassero la cornetta del telefono preparandosi a comporre o ricevere.
2. Il Chiamante ottiene l'accesso alla propria webcam e microfono (tramite il comando `getUserMedia`) e aggiunge questo flusso audio/video alla connessione. Sta dicendo: "Questi sono i dati che voglio inviare".
3. Il Chiamante genera un'**Offerta SDP**. È un file di testo che dice: _"Ciao, voglio connettermi. Supporto questi formati video, ho questo indirizzo IP, e voglio inviarti questo tipo di dati."_
4. Il Chiamante salva questa offerta all'interno della propria PeerConnection (tecnicamente si chiama `setLocalDescription`). Dice al proprio browser: "Ok, questa è la mia configurazione locale".
5. Il Chiamante invia questa offerta al Ricevente.
6. Il Ricevente ottiene il messaggio con l'offerta tramite il server di segnalazione.
7. Il Ricevente prende l'offerta del Chiamante e la salva nella propria PeerConnection (tecnicamente `setRemoteDescription`). Ora il Ricevente "sa" chi è il Chiamante e cosa è capace di fare.
8. Il Ricevente genera una **Risposta SDP**. Questo documento dice: _"Ho ricevuto la tua offerta. Io accetto e supporto questi formati, e il mio indirizzo IP è questo."_
9. Il Ricevente fa due cose, salva la propria risposta localmente (`setLocalDescription`) e invia la risposta indietro al Chiamante (sempre tramite il server di segnalazione).
10. Il Chiamante riceve la risposta del Ricevente e la salva nella propria PeerConnection (`setRemoteDescription`).