# BOB

## Introduzione

BOB è il nuovo software per la gestione dell'elettronica della bici, ha una struttura modulare composta da:

- MQTT broker
- moduli

### Server MQTT

MQTT (MQ Telemetry Transport or Message Queue Telemetry Transport) è un protocollo ISO standard (ISO/IEC PRF 20922)[2] di messaggistica leggero di tipo publish-subscribe posizionato in cima a TCP/IP.
Il broker è responsabile della distribuzione dei messaggi ai client destinatari. [Fonte](https://it.wikipedia.org/wiki/MQTT)

### Funzionamento dei moduli

I moduli hanno un main con queste caratteristiche in comune:

- inizializzano un'instanza della classe `Mqtt` o una sua derivata
- hanno un ciclo infinito in cui ad intervalli regolari o al verificarsi di eventi compie azioni
- hanno una funzione `handle_message` per gestire i segnali in arrivo o i messaggi a cui il modulo è iscritto
- estendono con una classe chiamata `Settings` la classe `CommonSettings`
- i sensori di ogni modulo devono estendere la classe `Sensor`

### Classe Mqtt

La classe Mqtt è una classe che si occupa di effettuare il collegamento al Mqtt broker e implementa i seguenti metodi:

- **costruttore** => Riceve come parametri l'ip del broker, la porta e il `nome del modulo`, l'oggetto di tipo `CommonSettings` del modulo che lo sta chiamando e un puntatore alla funzione del `message_handler`

- **on_connect** => Metodo chiamato nel momento in cui avviene la connessione al broker, effettua la subscribe ai topic `new_settings`, `signals`, `sensors/manager`.

- **on_message** => Metodo chiamato alla ricezione di un messaggio su un topic a cui si era effettuata l'iscrizione.

  - nel caso in cui il topic sia `new_settings` vengono aggiornate le impostazioni del modulo usando i metodi della classe `CommonSettings` e ripubblicate le impostazioni mandando anche il segnale `settings_updated`.
  - in tutti gli altri casi inoltra il messaggio al `message_handler`

- **subscribe** => Non fa nulla, serve solo per effettuare l'override.

- **publish** => Non fa nulla, serve solo per effettuare l'override.

- **publish_message** => Pubblica i settings sul topic `settings/{nome_modulo}`.

- **publish_settings** => Mostra un messaggio a schermo, pubblicando sul topic `messages/{nome_modulo}`.

- **publish_alert** => Crea una notifica da mandare al server, pubblicano un messaggio su `alerts/{nome_modulo}`.

La classe Mqtt viene estesa da:

- MqttSensor
- MqttMessage

#### MqttSensor

La classe MqttSensor estende Mqtt e viene usata nei moduli che **non devono ricevere dati da altri moduli** e pubblicare messaggi.

La classe MqttSensor aggiunge i seguenti metodi:

- **publish** => pubblica un messaggio con topic `sensors/{nome_modulo}`.

La classe MqttSensor è a sua volta estesa da MqttConsumer.

##### MqttConsumer

La classe MqttConsumer estende MqttSensor e viene usata nei moduli che **devono ricevere dati da altri moduli** e pubblicare messaggi.

La classe MqttConsumer aggiunge i seguenti metodi:

- **costruttore** => aggiunge al costruttore ereditato il campo `topics` che contiene la lista dei moduli su cui ci si vuole mettere in ascolto.

- **subscribe** => effettua l'override del metodo subscribe della classe Mqtt, il quale non effettuava nessuna azione, questo metodo viene chiamato dopo la connessione al broker ed effettua la subscrive all'elenco di moduli richiesti
- **subscribe_messages** => Effettua la subscribe al topic message **(todo: non ho la più pallida idea di chi la chiami)**

La classe MqttConsumer è a sua volta estesa dalla classe MqttRemote.

###### MqttRemote

La classe MqttConsumer estende MqttConsumer e viene usata nei moduli remoti che **devono modificare i settings o mandare i dati fuori dalla bici**.

La classe MqttRemote aggiunge i seguenti metodi:

- **subscribe** => effettua l'override chiamando la subscribe di MqttConsumer ed ed effettua la subscribe sui topic `settings/#` e `alerts/#` il cancelletto sta ad indicare che riceverà gli aggiornamenti ogni volta che verrà pubblicato un messaggio sui figli.

- **publish_signal** => permette di pubblicare segnali.

- **publish_new_settings** => permette di pubblicare le nuove impostazioni pubblicando sul topic `new_settings`, topic su cui tutti i moduli sono in ascolto.

- **publish_data** => Invio un dato di un sensore remoto, per esempio se vogliamo pubblicare i dati della stazione meteo usiamo questo metodo, pubblica dati sul sensore `sensors/{nome_sensore_remoto}`.

#### MqttMessage

La classe MqttMessage estende Mqtt e viene **usata esclusivamente nel modulo che mostra i messaggi a schermo**.

La classe MqttMessage aggiunge i seguenti metodi:

- **publish** => pubblica un messaggio con topic `messages`.
- **subscribe** => effettua la subscribe sul topic `messages/#` (il cancelletto sta ad indicare che riceverà gli aggiornamenti ogni volta che verrà pubblicato un messaggio sui figli).

## Installazione

### Installazione automatica

1. Scaricare e installare `Raspberry Pi OS Lite 32bit`
2. Scaricare BOB `git clone https://github.com/policumbent/BOB.git`
3. Avviare l'installazione `bash install.sh`

### Installazione manuale

1. Scaricare e installare `Raspberry Pi OS Lite 32bit`
2. Installare Docker
   1. `sudo apt update`
   2. `sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release -y`
   3. Aggiungere la chiave GPG ufficiale di Docker `curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`
   4. Aggiungo il repository `echo "deb [arch=armhf signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`
   5. `sudo apt update`
   6. `sudo apt install docker-ce docker-ce-cli containerd.io -y`
   7. docker-compose `sudo apt install docker-compose -y`
3. Installare portainer (facoltativo => per un più facile controllo dei container)
   1. `sudo docker volume create portainer_data`
   2. `sudo docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce`
   3. Impostare la password a `http://[ip_raspy]:9000`
4. Installare BOB
   1. Installare git `sudo apt install git -y`
   2. Scaricare BOB `git clone https://github.com/policumbent/BOB.git`
   3. Andare nella cartella di BOB `cd BOB`
   4. Copiare i fle in comune `python3 copy_common.py`
   5. Buildare i container `sudo docker-compose build`
   6. Installare le dipendenze per il bluetooth => [vedi qui](https://docs.policumbent.it/#/software/BOB?id=dipendenze-senza-docker)
   7. Aggiungere il servizio d'avvio `sudo cp BOB.service /etc/systemd/system/`
   8. `sudo systemctl enable BOB.service`
   9. `sudo systemctl start BOB.service`
5. Installare splash screen
   1. `sudo apt install fbi -y`
   2. `sudo cp splashscreen.service /etc/systemd/system/`
   3. `sudo systemctl enable splashscreen.service`
   4. `sudo systemctl start splashscreen.service`
   5. Controllare lo stato `sudo systemctl status splashscreen.service`

## Elenco moduli

### Gpio

Il modulo GPIO si occupa della gestione degli interrupt provenienti dai tasti.
Alla pressione dei tasti vengono pubblicati diversi messaggi:

- `1` => Tasto up (pin 27) premuto
- `-1` => Tasto down (pin 17) premuto

Nel caso in cui si verifichi una determinata combinazione di tasti, entrambi i tasti premuti contemporanemente, verrà pubblicato il **segnale** `reset`.

#### Tipologia

Il modulo GPIO è un MqttRemote.

#### Dipendenze

_Work in progress_

#### Impostazioni

Il modulo GPIO non richiede impostazioni particolari.

#### Segnali inviati

- `reset`

#### Segnali ricevuti

Il modulo GPIO non esegue azioni alla ricezione di nessun segnale.

#### Notifiche

Il modulo GPIO non manda notifiche.

#### Messaggi a schermo

Il modulo GPIO non manda messaggi a schermo.

### Hall sensor

Secondo sensore hall sulla bici che si occupa di misurarne la distanza e la velocità tramite l'utilizzo di un magnete presente sula ruota.

#### Tipologia

Il modulo GPIO è un MqttSensor.

#### Dipendenze

- paho-mqtt
- RPi.GPIO

#### Impostazioni

- pin (default 24) ==> pin signal hall sensor
- circunference (default 1450) ==> cinrconferenza in millimetri della ruota

#### Segnali ricevuti

- `reset` ==> resetta la distanza percorsa

#### Notifiche

Il sensore hall non manda notifiche.

#### Messaggi a schermo

Il sensore hall non manda messaggi a schermo.

### Csv

Il modulo csv si occupa di loggare su un file csv i dati provenienti dai sensori selezionati. All'avvio il modulo si iscrive all'elenco dei topic che deve loggare e ogni x secondi scrive i dati su un file.
I file creati dal modulo hanno questo formato `{nome_bici}-{timestamp}.csv`.

#### Tipologia

Il modulo csv è un consumatore.

#### Dipendenze

_Work in progress_

#### Impostazioni

- `csv_Save_interval` => ogni quanto i dati vengono salvati sul csv es. `1` (1s)

#### Segnali ricevuti

- `reset` => alla ricezione del segnale reset deve chiudere il file e aprirne uno nuovo

#### Notifiche

Il modulo csv non manda notifiche.

#### Messaggi a schermo

Il modulo csv non manda messaggi a schermo.

### Bluetooth

#### Dipendenze (senza docker)

1. `sudo apt update`
2. `sudo apt-get install bluetooth bluez libbluetooth-dev`
3. `sudo python3 -m pip install pybluez`
4. Modificare la riga `ExecStart=/usr/lib/bluetooth/bluetoothd` in `ExecStart=/usr/lib/bluetooth/bluetoothd --compat` nel file `/etc/systemd/system/dbus-org.bluez.service`.

### Gps

Nam condimentum aliquam neque at dignissim. Praesent suscipit massa a elit congue mollis. Interdum et malesuada fames ac ante ipsum primis in faucibus. Aenean sed iaculis metus. Integer tempor augue nec porta feugiat. Nulla eleifend sed dui quis maximus. Sed rutrum fermentum orci at hendrerit. Sed suscipit tempor massa, eget pellentesque tellus sagittis a. Pellentesque dapibus gravida sem, sit amet cursus lacus eleifend ac. Nullam lobortis tristique nisl, id pulvinar libero molestie vitae. Donec porttitor ultricies dui, sed pretium libero fringilla et. Sed dictum blandit enim, vel euismod magna viverra at. Integer mi velit, iaculis ornare porta at, placerat vel nibh. Phasellus dictum, sem eget tempus ultricies, enim turpis dignissim ante, sit amet porta sapien tortor at ligula.

### Ant

Phasellus porta sit amet magna ut rhoncus. Fusce congue purus lacus, in facilisis enim posuere ut. Nulla fermentum leo mi, sit amet consequat metus placerat sit amet. Sed pellentesque in dui et pharetra. Donec non mauris at nulla viverra posuere eget id purus. Aliquam fringilla tincidunt ante eu congue. Aliquam erat volutpat. Donec id venenatis leo. Phasellus vitae consectetur nisi. Fusce dictum consectetur ex eget placerat.

### Communication

Phasellus porta sit amet magna ut rhoncus. Fusce congue purus lacus, in facilisis enim posuere ut. Nulla fermentum leo mi, sit amet consequat metus placerat sit amet. Sed pellentesque in dui et pharetra. Donec non mauris at nulla viverra posuere eget id purus. Aliquam fringilla tincidunt ante eu congue. Aliquam erat volutpat. Donec id venenatis leo. Phasellus vitae consectetur nisi. Fusce dictum consectetur ex eget placerat.

### Accelerometro MPU6050

#### Dipendenze

https://github.com/Tijndagamer/mpu6050.git

#### Aumento della frequenza di campionamento (per migliorare le prestazioni)

Aggiungere la linea:
`dtparam=i2c_arm=on,i2c_arm_baudrate=400000`
nel file:
`boot/config.txt`

#### Controllare la connessione i2c

Il codice assume che il sensore sia connesso all'iindirizzo 0x68.
Per verificare che ciò sia vero:
`i2cdetect -y 1`

#### PIN MPU6050 (PIN sensore -> GPIO Pi4)

- VCC -> 3v3 power (PIN 1)
- GND -> GND (PIN 9)
- SCL -> GPIO3 (PIN 5)
- SDA -> GPIO2 (PIN 3)
