# Sofia


## Protocollo

1. Sono connesso alla rete della bici? Se si uso quella, altrimenti uso il bt

1. Apertura connessione => All'apertura dell'app o alla pressione del tasto connetti

1. BICI => APP
    - all'apertura: manda settings e la roba che deve
    - ogni volta che i dati vengono aggiornati: manda settings e la roba che deve

1. APP => BICI
    - ogni volta modifichiamo qualcosa mandiamo il pacchetto tipo 1

1. Chiusura connessione => Alla chiusura dell'app

```js
{
    'type': 'signal',
    'data': 'reset'
}

{
    'type': 'settings',
    'data': {
        'max_temp': 71,
        'max_temp': 71
    }
}

{
    'type': 'gear',
    'data': {
        'max_temp': 71,
        'max_temp': 71
    }
}
```
*pacchetto tipo 1*

```js
{
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
}
```
Esempio delle impostazioni che servono
