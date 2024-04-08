# Stazione meteo

La stazione meteo basa il suo funzionamento su un ESP-WROOM-32: un microcontrollore della famiglia Espressif che ha al suo interno un modulo Wifi.
Tal modulo le permette di inviare dati in tempo reale ad un server MQTT, dal quale noi possiamo visualizzarli.

La stazione meteo è dotata di diversi sensori che le permettono di fornire dati riguardo:

    1. Temperatura (°C)       --> BME280
    2. Pressione (Pa)         --> BME280
    3. Umidità relativa (%)   --> BME280
    4. Velocità del vento (Km/h)   --> Anemometro
    5. Direzione del vento (Gradi) --> Encoder Magnetico Rotativo

## Alimentazione

L'alimentazione è esterna fornita da un powerbank che risiede all'interno della stazione.
Il Powerbank fornische un voltaggio di 5V che alimenta direttamente ESP ed Anemometro.
Tutti gli altri sensori ed i bus SDA/SCK ricevono una tensione di 3.3V.

| Caratteristica       | Valore               |
| -------------------- | -------------------- |
| **Modello**          | Powerbank anker nome |
| **Link datasheet**   |                      |
| **Link acquisto**    |                      |
| **Corrente output**  | 2A                   |
| **Capacità**         | 20Ah                 |
| **Voltaggio outupt** | 5V                   |

## Convertitore Back DC-DC regolabile

Serve ad aumentare il voltaggio. Risulta necessario dal momento che l'anemometro opera tra gli 8 e 12 volt.

| Caratteristica       | Valore                          |
| -------------------- | ------------------------------- |
| **Modello**          | Convertitore step up Back DC-DC |
| **Link datasheet**   |                                 |
| **Link acquisto**    |                                 |
| **Voltaggio input**  | 3 - 32V                         |
| **Voltaggio outupt** | 5 - 40V                         |

## Anemometro

Il sensore che misura la velocità del vento.
Aumentando la velocità di rotazione, aumenta il voltaggio in uscita verso l' ADC.

| Caratteristica         | Valore                                                                 |
| ---------------------- | ---------------------------------------------------------------------- |
| **Modello**            | Adafruit Anemometer                                                    |
| **Link datasheet**     | https://cdn-shop.adafruit.com/product-files/1733/C2192%2Bdatasheet.pdf |
| **Link acquisto**      |                                                                        |
| **Uscita dati**        | Valore di tensione in base alla velocità del vento (0.4V - 2.0V)       |
| **Velocità minima**    | 0 Km/h                                                                 |
| **Velocità massima**   | 32 Km/h                                                                |
| **Voltaggio in input** | 8 - 12V                                                                |

## BME280

Sensore che misura Temperatura, Umidità e pressione dell'aria. Comunica in I2C direttamente con l' ESP.

| Caratteristica         | Valore                                                                                                                    |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **Modello**            | CJMCU BME 280                                                                                                             |
| **Link datasheet**     | https://cdn-learn.adafruit.com/downloads/pdf/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout.pdf |
| **Link acquisto**      |                                                                                                                           |
| **Uscita dati**        | Trasmette attraverso SDA/SCK BUS con protocollo I2C                                                                       |
| **Voltaggio in input** | 3.3V                                                                                                                      |

## Encoder Magnetico Rotativo

Il sensore che misura la direzione del vento in gradi. Sotto la bandierina è montato un piccolo magnete polarizzato diagonalmente.
Il sensore misura l'angolo tra il Nord del magnete e il suo asse di riferimento.

| Caratteristica         | Valore                                                         |
| ---------------------- | -------------------------------------------------------------- |
| **Modello**            | Adafruit Anemometer                                            |
| **Link datasheet**     | https://ams.com/documents/20143/36005/AS5048_DS000298_4-00.pdf |
| **Link acquisto**      |                                                                |
| **Uscita dati**        | Comunica con protocollo SPI attraverso bus MOSI/MISO           |
| **Voltaggio in input** | 3.3V                                                           |

## ADC

l' Analog to Digital Converter converte la tensione in input sul pin A0 ricevuta dall'anemometro in un valore digitale che poi trasferisce all' ESP
attraverso protocollo I2C

| Caratteristica         | Valore                                               |
| ---------------------- | ---------------------------------------------------- |
| **Modello**            | Adafruit ADS1115                                     |
| **Link datasheet**     | https://cdn-shop.adafruit.com/datasheets/ads1115.pdf |
| **Link acquisto**      |                                                      |
| **Uscita dati**        | Comunica con protocollo I2C attraverso bus SDA/SCK   |
| **Bit Precision**      | 16 bit                                               |
| **Voltaggio in input** | 3.3V                                                 |

## RTC

Il Real Time Clock è un modulo che mantiene attivo un clock all'interno della stazione in modo tale da avere informazioni utili riguardo i tempi di raccolta dei dati.

| Caratteristica         | Valore                                                                                  |
| ---------------------- | --------------------------------------------------------------------------------------- |
| **Modello**            | Adafruit ADS1115                                                                        |
| **Link datasheet**     | https://cdn-learn.adafruit.com/downloads/pdf/adafruit-ds3231-precision-rtc-breakout.pdf |
| **Link acquisto**      |                                                                                         |
| **Uscita dati**        | Comunica con protocollo I2C attraverso bus SDA/SCK                                      |
| **Voltaggio in input** | 3.3V                                                                                    |
| **Batteria Supporto**  | CR 2032 3.3V                                                                            |

## SD READER

La stazione meteo monta una scheda SD per il salvataggio dei dati in locale(supporto necessario in caso di scarsa connessione).
Il modulo ha il solo scopo di interfacciarsi con la scheda SD.

| Caratteristica         | Valore                                                                                  |
| ---------------------- | --------------------------------------------------------------------------------------- |
| **Modello**            | Micro SD Card Adapter                                                                   |
| **Link datasheet**     | https://cdn-learn.adafruit.com/downloads/pdf/adafruit-ds3231-precision-rtc-breakout.pdf |
| **Link acquisto**      |                                                                                         |
| **Uscita dati**        | Comunica con protocollo SPI attraverso bus MOSI/MISO                                    |
| **Voltaggio in input** | 3.3V                                                                                    |
| **Dimensione massima** | SD (2 Gb) SDHC (32 Gb)                                                                  |
| **File System**        | FAT                                                                                     |
