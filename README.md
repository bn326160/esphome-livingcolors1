This is a fork of [esphome-livingcolors1](https://github.com/rrooggiieerr/esphome-livingcolors1) by [@rrooggiieerr](https://github.com/rrooggiieerr).
I modified the code so the command is sent twice each time, that was necessary in [my case](https://github.com/rrooggiieerr/esphome-livingcolors1/issues/2#issuecomment-2307271623).

# esphome-livingcolors1
Philips LivingColors 1st generation component for ESPHome.

To set up this LivingColors component you first need to place a top-level SPI and CC2500 component which defines the pins to use.

## Hardware required
- ESP8266 or ESP32
- CC2500 transceiver

These need to be wired as described on the [ESPHome CC2500 component](https://github.com/rrooggiieerr/esphome-cc2500)

![](wiring3.jpg)

## Getting the address
1. Install and Run [Phil](https://github.com/philipplive)'s [Philips Living Colors (Gen 1) mit NodeMCU (ESP8266) + CC2500 steuern](https://github.com/philipplive/Philips-Living-Colors-Gen1-with-NodeMCU) after [fixing the code](https://github.com/rrooggiieerr/esphome-livingcolors1/issues/2#issuecomment-1938948705) by returning a bool value at the end of the addLamp() function.
2. Keep the serial monitor on and press a button on your remote.
3. Look at the output after "Vorhandene Adressen im Speicher:", this lists the address(es) of your lights in decimal format.
4. [Convert](https://www.convzone.com/decimal-to-hex/) the decimal output to hex
5. Make sure each digit is converted to 2 hex characters (See https://github.com/rrooggiieerr/esphome-livingcolors1/issues/2#issuecomment-2275723270)
6. Remove the spaces and prefix the 16 character hex string with a 0x

## ESPHome example configuration:
```
esphome:
  name: livingcolors1

external_components:
  - source:
      type: git
      url: https://github.com/rrooggiieerr/esphome-cc2500
      ref: 0.0.2
    components: [cc2500]
  - source:
      type: git
      url: https://github.com/bn326160/esphome-livingcolors1
      ref: 0.0.3
    components: [livingcolors1]

esp8266:
  board: nodemcu

# Enable logging
logger:
  level: DEBUG

spi:
  clk_pin: GPIO14
  mosi_pin: GPIO13
  miso_pin: GPIO12

cc2500:
  cs_pin: GPIO15
  output_power: 0xFF

livingcolors1:

light:
  - platform: livingcolors1
    name: "LivingColors One"
    address: 0x...
  #- platform: livingcolors1
  #  name: "LivingColors Two"
  #  address: 0x...

wifi:
  ssid: ""
  password: ""

api:
  password: ""

web_server:
  port: 80
  version: 3
```

## Credits
- [Phil](https://github.com/philipplive)'s [Philips Living Colors (Gen 1) mit NodeMCU (ESP8266) + CC2500 steuern](https://github.com/philipplive/Philips-Living-Colors-Gen1-with-NodeMCU) was helpful for making this component.
- [@rrooggiieerr](https://github.com/rrooggiieerr)'s [Philips LivingColors 1st generation component for ESPHome](https://github.com/rrooggiieerr/esphome-livingcolors1) by [@rrooggiieerr](https://github.com/rrooggiieerr)
