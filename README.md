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
* [CP210X Drivers](https://www.silabs.com/products/development-tools/software/interface). I used the OSX version. This is needed for communication with the ESP8266 via USB.
* [Atom](https://atom.io/) - Optinal.
* [PlatformIO Addon](https://platformio.org/). If no Atom, you need some other way of using this.
* [Arduino IDE](https://www.arduino.cc/en/Main/Software).
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
3. Open PlatformIO in Atom.
4. Open PlatformIO terminal.
   * In Atom, choose the menu `Packages > platformio-ide > New Terminal`.
   * `cd` to the directory where you have cloned [this repo](https://github.com/selfhostedhome/smart-bed-sensor).
   * Connect the ESP8266 to your computer running Atom, and run (in PlatmformIO terminal within Atom):
   ```bash
   platformio run --target upload
   ```
OBS! I cannot find the ESP8266 in the serial monitor. So cannot upload...? How to fix??

## Home Assistant

I am using this in Home Assistant, though it could be used in a plethora of ways. See my Home Assistant repository for more info, but basically I set up an MQTT sensor in my `sensors.yaml` to get the data (which is raw data, not converted to weight):

```yaml
- platform: mqtt
  state_topic: 'home/bedroom/bed'
  name: "Raw Master Bed Weight Measurement"
  ```
And a template sensor (also in `sensors.yaml`) to determine who is in bed. This is just based on cutoff values for different weights.

```yaml
- platform: template
  sensors:
    person_1_in_bed:
      friendly_name: "Person 1 in Bed"
      value_template: >
        {{ states('sensor.raw_master_bed_weight_measurement')|float >= XXXXXX }}
    person_2_in_bed:
      friendly_name: "Person 2 in Bed"
      value_template: >
        {{ states('sensor.raw_master_bed_weight_measurement')|float > XXXXXX
          and (states('sensor.raw_master_bed_weight_measurement')|float < XXXXXX
               or states('sensor.raw_master_bed_weight_measurement')|float >= XXXXXX)}}
```


___


<nowiki>*</nowiki> I use Mosquitto running in Docker on the computer also running Home Assistant. See my [Home Assistant repository](https://github.com/Aephir/Home_Assistant) for more details on this.
