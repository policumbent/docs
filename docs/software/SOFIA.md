# Sofia

## Protocollo

1. Sono connesso alla rete della bici? Se si uso quella, altrimenti uso il bt

1. Apertura connessione => All'apertura dell'app o alla pressione del tasto connetti

1. BICI => APP

   - all'apertura: manda il _pacchetto 2_.
   - ogni volta che i settings vengono aggiornati: manda il _pacchetto 2_ a tutti i client connessi.
   - ogni volta che viene ricevuto un segnale: manda il _pacchetto 3_ a tutti i client connessi.

1. APP => BICI

   - ogni volta modifichiamo qualche settings o vogliamo mandare il segnale mandiamo il _pacchetto tipo 1_.

1. Chiusura connessione => Alla chiusura dell'app.

(_pacchetto tipo 1_)

```json
{
  "type": "signal",
  "data": "reset",
  "key_digest": ""
}

{
  "type": "settings",
  "data": {
    "max_temp": 71,
    "run_length": 8000
  },
  "key_digest": ""
}

{
  "type": "gear",
  "data": {
    "servo_1_up": [71, 54, 64, 34, 44, 33, 44, 32, 22, 9, 2],
    "servo_2_up": [0, 2, 3, 33, 44, 55, 66, 77, 88, 99, 120],
    "servo_1_down": [71, 54, 64, 34, 44, 33, 44, 32, 22, 9, 2],
    "servo_2_down": [0, 2, 3, 33, 44, 55, 66, 77, 88, 99, 120]
  },
  "key_digest": ""
}
```

(_pacchetto tipo 2_)

```json
{
  "settings": {
    "settings/gpio": {},
    "settings/manager": {
      "max_temp": 70,
      "autopause": false,
      "bike": "taurusx",
      "autopause_on_gps": false
    },
    "settings/gps": {
      "latitude_timing_start": 45.032888,
      "longitude_timing_start": 7.792347,
      "latitude_timing_end": 45.032888,
      "longitude_timing_end": 7.792347
    },
    "settings/messages": {
      "trap_priority": 2
    },
    "settings/http_service": {
      "server_ip": "poliserver.duckdns.org",
      "cert": "./cert.crt",
      "server_port": 9002,
      "protocol": "https",
      "username": "admin",
      "password": "admin"
    },
    "settings/ant": {
      "hour_record": false,
      "run_length": 8046,
      "trap_length": 200,
      "hr_sensor_id": 0,
      "speed_sensor_id": 0,
      "power_sensor_id": 0,
      "circumference": 1450,
      "average_power_time": 3
    },
    "settings/accelerometer": {
      "accelerometer_local_csv": false
    },
    "settings/bt": {}
  },
  "incremental_number": 1
}
```

(_pacchetto tipo 3_)

```json
{
  "signal": "reset",
  "incremental_number": 1
}
```

## Tool di sviluppo

### Live reloading

Per abilitare il live reloading dell'app:

1. Avviare il webserver con il live reloading`vue-cli-service serve`

2. `ionic capacitor run android --livereload-url=http://{ip_pc}:8080/`

3. Avviare l'app da android.

### Build app

1. `ionic build`
2. `npx cap sync`
3. `ionic capacitor run android`

Se non funziona cancellare le opzioni del server dal file `capacitor.config.json`.
