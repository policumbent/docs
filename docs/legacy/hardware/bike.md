# Elettronica bici

## COLLEGAMENTO RASPY

- sensore hall
  - gnd
  - 3v3
  - pin 18
- accelerometro
  - 3v3
  - gnd
  - sda => 3
  - scl => 5
- meteo
  - vcc -> 3v3 power (PIN 1)
  - GND -> GND (PIN 9)
  - SDI -> GPIO2 (PIN 3)
  - SCK -> GPIO3 (PIN 5)
  - CSB -> 3v3 power (PIN 1)
  - SDO -> GND (PIN 9)
- gps
  - 27 => rx
  - 28 => tx
  - 5v
  - gnd
- up and down
  - 11 => down
  - 13 => up
- arduino comunicazione
  - rx => 8
  - tx => 10
- lora/xbee
  - 24
  - 21
- seriale libera
  - 32
  - 33
- gpio libere
  - 15 => tasto
  - 19 => tasto
  - 23 => diretta
- led
  - 16
  - 5v diretto dal buck
  - gnd
- usb come nella vecchia scheda

## collettore schermi

- sensore meteo
- xbee/lora
- led
- un tasto (15)
- gps

## piastre MAIN BOARD

- socket arduino nano => da comprare
- accelerometro
- sensore hall
- gpio libere e seriale libera
- tasti cambio
