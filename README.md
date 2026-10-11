# MQTT Broker-Bridge-Client

App Android per usare un broker MQTT locale, collegarsi a broker MQTT esistenti e inoltrare messaggi tra due broker con regole di mappatura.

## Download

Scarica e installa l'ultima versione disponibile:

[Scarica MQTT Broker-Bridge-Client v1.3](./MQTT-Broker-Bridge-Client-v1.3.apk)

Richiede **Android 7.0 (API 24) o successivo**.

Per installare l'APK, apri il file sul dispositivo Android e segui le istruzioni. Se richiesto, consenti al browser o all'app usata per il download di installare app da questa origine.

## Funzioni

- Broker MQTT locale con porta TCP configurabile e opzione SSL/TLS.
- Client MQTT per sottoscrivere topic, pubblicare messaggi e visualizzare la cronologia.
- Bridge unidirezionale o bidirezionale tra broker MQTT.
- Regole per trasformare topic e payload nel percorso sorgente → destinazione.
- Backup e ripristino della configurazione in formato JSON.
- Notifiche separate per Broker, Client e Bridge.

## Avvio rapido

Per provare l'app sullo stesso dispositivo:

1. In **Configurazione Broker**, lascia la porta TCP **1883** e avvia il broker.
2. In **Configurazione Bridge**, imposta come sorgente e destinazione lo stesso broker locale:
   - URL: **tcp://127.0.0.1:1883**
   - ID client sorgente: **BridgeSrc**
   - ID client destinazione: **BridgeDst**
   - Topic sorgente: **demo/input**
   - Topic destinazione: **demo/output**
3. Per questa prova, disattiva **Bridge Bidirezionale** e avvia il bridge.
4. In **Mappatura messaggi**, aggiungi una regola:
   - Topic ingresso: **demo/input**
   - Payload ingresso: **evento_1**
   - Topic uscita: **demo/output**
   - Payload in uscita: **evento_1_completo**
5. In **Configurazione Client**, usa l'URL **tcp://127.0.0.1:1883**, imposta il topic di ascolto su **demo/output** e avvia il client.
6. Nella schermata Home pubblica il payload **evento_1** sul topic **demo/input**.

Il client dovrebbe ricevere **evento_1_completo** sul topic **demo/output**. Per provare l'inoltro senza mappatura, pubblica un payload diverso da **evento_1**: verrà inoltrato invariato.

## Collegare un dispositivo esterno

Per connettere un dispositivo MQTT al broker in esecuzione sul telefono:

1. Collega telefono e dispositivo alla stessa rete, oppure assicurati che la rete renda il broker raggiungibile.
2. Leggi l'indirizzo IPv4 mostrato nella Home.
3. Configura il client esterno con quell'indirizzo e la porta del broker, ad esempio **tcp://192.168.1.50:1883**.
4. Pubblica o sottoscrivi i topic configurati nell'app, rispettando maiuscole e minuscole.

**127.0.0.1** identifica sempre il dispositivo locale: per un dispositivo esterno usa l'indirizzo raggiungibile del telefono.

## Bridge bidirezionale

Attiva **Bridge Bidirezionale** per inoltrare anche dal topic di destinazione al topic sorgente principale. In modalità bidirezionale:

- Il topic sorgente deve essere concreto, senza wildcard MQTT **+** o **#**.
- Se sorgente e destinazione si collegano allo stesso broker, usa ID client differenti.
- Le regole di mappatura si applicano soltanto dal topic sorgente alla destinazione. Il percorso inverso inoltra il payload senza trasformarlo.

## Regole di mappatura

- I topic distinguono maiuscole e minuscole.
- Il payload viene confrontato senza distinzione tra maiuscole e minuscole e ignorando spazi all'inizio e alla fine.
- Vince la prima regola abilitata che corrisponde.
- Le wildcard MQTT **+** e **#** sono supportate nei topic di ingresso delle regole.
- Se non corrisponde alcuna regola, il messaggio viene inoltrato invariato sul topic di destinazione configurato.

## Notifiche e funzionamento in background

La schermata **Protezione da chiusura** offre collegamenti alle impostazioni Android separate per **Broker**, **Client** e **Bridge**. Da lì puoi regolare visibilità, suoni e avvisi per ogni categoria di notifica.

Android e alcuni produttori possono limitare l'attività in background. Per mantenere i servizi attivi, controlla anche l'esclusione dalle ottimizzazioni della batteria e, se il dispositivo la offre, l'impostazione di avvio automatico.

## Backup e credenziali

Puoi salvare la configurazione sul dispositivo oppure esportarla e ripristinarla da un file JSON.

Il backup contiene le impostazioni e le credenziali MQTT **in chiaro**. Salvalo e condividilo solo in luoghi fidati; elimina o proteggi le password prima di inviare il file ad altre persone.

## Sicurezza della rete

Le impostazioni predefinite usano TCP e non cifrano il traffico. Usa broker e reti fidati. Se esponi un broker locale ad altri dispositivi, controlla le opzioni di accesso del broker e della rete prima di usarlo.
