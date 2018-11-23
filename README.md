# Bed_occupancy_sensors
Bed occupancy sensor using load cells, HX711, and ESP8266. This transmits over MQTT.

Mainly from this: https://selfhostedhome.com/diy-bed-presence-detection-home-assistant/

## Hardware

* [50 kg load cells (4 pcs) and HX711](https://www.amazon.co.uk/dp/B07H4L36VR/ref=pe_3187911_185740111_TE_item).
* [ESP8266 ESP-12E CP2102](https://www.amazon.co.uk/dp/B078NZGFHT/ref=pe_3187911_185740111_TE_item).
* Wires.
* Isotape.

## Tools
* Solering Iron & tin.
* Tweesers and other generic small electronics tools.
* USB to microUSB cable.

## Software
* [Atom](https://atom.io/) - Optinal.
* [PlatformIO Addon](https://platformio.org/). If no Atom, you need some other way of using this.
* [Arduino IDE](https://www.arduino.cc/en/Main/Software).

## Other
* MQTT broker*.

## Guide

### Assembly
Assmeble the six parts as shown below. Do it however you want for testing, but make sure the wires are long enough that you can fit one under each leg of the bed when you get ready to actually deploy.

1. The HX711 and ESP8266 are connected by four wires (VCC, GND and 2x data)
2. The four load cells are connected in serial, black wire to black wire and white wire to white wire.
3. Each of the four load cells are connected to the HX711 via their red wire.

![Connections](https://github.com/Aephir/Bed_occupancy_sensors/blob/master/Connections.svg)


### Software 
With the software listed already installed:

1. Clone [this repo](https://github.com/selfhostedhome/smart-bed-sensor).
2. Change the `config_template.h` file to include your 
   * WiFi SSID within the quotes (").
   * WiFi password within the quotes (").
   * MQTT server (IP address; I use my local IP, since this is all running within my LAN). Should I include the port??
3. Open PlatformIO in Atom, add your project
   * Choose `Espressif ESP8266 ESP-12E` board.
   * Choose `???` framework.
   * Choose location.
4. Open PlatformIO terminal.
   * In Atom, choose the menu `Packages > platformio-ide > New Terminal`.
   * `cd` to the directory where you have cloned [this repo](https://github.com/selfhostedhome/smart-bed-sensor).
   * Connect the ESP8266 to your computer running Atom, and run (in PlatmformIO terminal within Atom):
   ```bash
   platformio run --target upload
   ```
___


<nowiki>*</nowiki> I use Mosquitto running in Docker on the computer also running Home Assistant. See my [Home Assistant repository](https://github.com/Aephir/Home_Assistant) for more details on this.
