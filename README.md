# Maxi Slave ESP32-S3

Firmware per **ESP32-S3** sviluppato in **Visual Studio Code** con **PlatformIO** e framework **Arduino** (ambiente Linux Mint).

## Obiettivo

La scheda ESP32-S3 acquisisce dati da più sensori collegati su bus differenti, li elabora e li inoltra a un dispositivo master tramite una UART dedicata, usando pacchetti in formato NMEA.

## Architettura funzionale

- **I2C**: acquisizione di sensori collegati su bus I2C.
- **UART + Modbus**: acquisizione di sensori seriali che parlano protocollo Modbus.
- **UART GPS (GNSS)**: ricezione delle stringhe NMEA originali del modulo GPS.
- **UART verso Master**: trasmissione dati in uscita:
  - ripetizione dei pacchetti NMEA ricevuti dal GPS;
  - aggiunta di pacchetti NMEA custom con dati dei sensori non-GPS.

## Flusso dati

1. Lettura periodica dei sensori su I2C.
2. Polling/interrogazione dei sensori Modbus su UART.
3. Acquisizione continua delle frasi NMEA del modulo GNSS.
4. Analisi/normalizzazione dei valori misurati.
5. Composizione dei pacchetti NMEA custom per il master.
6. Invio seriale su UART di:
   - frasi GPS originali;
   - frasi custom con i dati aggregati dei sensori.

## Note di progetto

- Il design è orientato a integrare dati eterogenei (I2C, Modbus, GNSS) in una singola interfaccia seriale standardizzata.
- L'uso del formato NMEA consente integrazione semplice con sistemi master già compatibili con parser NMEA.
