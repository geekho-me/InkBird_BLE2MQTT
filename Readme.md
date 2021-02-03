# iBBQ-Gateway 

an ESP32 Based iBBQ (Inkbird) BLE to MQTT Gateway

## Inkbird Compatbility:

* Inkbird IBT-6X
* [Inkbird IBT-6XS (Amazon UK Affiliate Link)](https://amzn.to/3t2Rymx)

## ESP32 Compatbility:

* [Wemos LOLIN32 Lite (Amazon UK Affiliate Link)](https://amzn.to/3t2QT4x)

## Features:

* Automatically connects to iBBQ Bluetooth BBQ thermometers
* Adapts amount of channels to connected thermometer
* Should work with most ESP32 boards available
* Should work with iBBQ based Bluetooth BBQ thermometers with up to 8 channels
* Publish temperature (Celsius) as MQTT Topic
* Publish battery level as MQTT Topic
* Automatically reconnects to iBBQ BLE if disconnected

## Requirements:

* Rename the `credentials.example.h` to `credentials.h` and edit it to your needs

## Usage:

* After flashing the device will automaitcally connect to your WiFi, connect to your iBBQ Device and then start publishing data to MQTT

## MQTT Topics:

* `inkbird/BLE` - Reports if the device is `Connected` or `Disconnected` to your iBBQ
* `inkbird/BATTERYLEVEL` - Reports the iBBQ Battery Level (e.g. `73` equals 73%)
* `inkbord/PROBE1` - Reports the temperature of Probe 1 (e.g `120` equals 120c)
  * Each Probe has its own topic (e.g. `inkbord/PROBE1`, `inkbord/PROBE2`, `inkbord/PROBE3`, etc)

## Known Issues:

* The `ESP32 BLE Arduino` library has a bug in the connection timeout:
  * Workaround: Make the following changes to `FreeRTOS.cpp`:
  * Change: `xSemaphoreTake(m_semaphore, portMAX_DELAY);` to `xSemaphoreTake(m_semaphore, 15000UL);`
  * Change: `rc = ::xSemaphoreTake(m_semaphore, portMAX_DELAY) == pdTRUE;` to `rc = ::xSemaphoreTake(m_semaphore, 15000UL) == pdTRUE;`

## Todo:

- [ ] Improve the MQTT stability
- [ ] More verbose status logging to MQTT
- [ ] Change the ESP32 BLE library to something more modern?

## References:

* Protocol Description: https://gist.github.com/uucidl/b9c60b6d36d8080d085a8e3310621d64
* Battery Status: https://github.com/sworisbreathing/go-ibbq/issues/2#issuecomment-650725433
* ESP32 BLE Timeout Bug: https://github.com/nkolban/esp32-snippets/issues/874