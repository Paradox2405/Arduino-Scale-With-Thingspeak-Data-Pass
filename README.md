# Arduino Wi-Fi Scale with ThingSpeak

An IoT weighing scale. An Arduino reads a load cell through an **HX711** amplifier and sends each reading over Wi-Fi with an **ESP8266** module to a **ThingSpeak** channel, where it's logged and charted in the cloud.

## Hardware

| Part | Notes |
|---|---|
| Arduino (Uno/Nano) | runs `esp4/esp4.ino` |
| Load cell + HX711 amplifier | `DOUT` → pin 5, `SCK` → pin 4 |
| ESP8266 (ESP-01) | SoftwareSerial `RX` → pin 2, `TX` → pin 3, 115200 baud |

## How it works

1. On boot the scale is **tared** and the calibration factor is applied (`-90000`, worked out with the SparkFun HX711 calibration sketch).
2. The ESP8266 joins your Wi-Fi with AT commands (`AT+CWMODE=1`, `AT+CWJAP`).
3. Each loop reads the weight in kg and sends an HTTP `GET /update?api_key=…&field1=<kg>` to `api.thingspeak.com`.

## Setup

1. Install the **HX711** library in the Arduino IDE.
2. Edit the settings at the top of `esp4/esp4.ino`:
   ```cpp
   String AP   = "YOUR_WIFI_SSID";
   String PASS = "YOUR_WIFI_PASSWORD";
   String API  = "YOUR_THINGSPEAK_WRITE_KEY";
   ```
3. Calibrate your load cell and update `calibration_factor`.
4. Upload, open the Serial Monitor at 9600 baud, and watch the readings arrive on your ThingSpeak channel.
