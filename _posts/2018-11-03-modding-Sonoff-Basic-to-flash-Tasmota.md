---
layout: post
title: "Modding Sonoff Basic to flash Tasmota"
date: 2018-11-03
categories: [blog]
tags: [sonoff, tasmota]

---
# Modding Sonoff Basic for Tasmota


| Parts               | Qty  |                                                             |
| ------------------- | :---: | ----------------------------------------------------------- |
| Jumper pins         | 1    | ![](/images/5_pins_jumper.jpg) |
| 1K Ohm SMD resistor | 1    | ![](https://carbonpanda.github.io/images/1K_resistor.jpg)   |


Steps:

1. Solder the five pins jumper to the Sonoff board

   ![5_pins_jumper_placement](https://carbonpanda.github.io/images/5_pins_jumper_placement.jpg)

   ![5_pins_jumper_soldered](https://carbonpanda.github.io/images/5_pins_jumper_soldered.jpg)

2. Solder the 1K Ohm resistor to the Ground and GPIO14 pin. This is based on [DrZZs suggestion](http://drzzs.com/zzonoff-my-masterpiece-diy-smart-switch/) otherwise there will be too much noise or interference when a switch is connected to GPIO14.

   ![1K_resister_soldered](https://carbonpanda.github.io/images/1K_resister_soldered.jpg)


# Flashing Tasmota

Steps:

1. Follow [DrZZs updated flashing instruction](http://drzzs.com/updated-sonoff-basic-flashing/) to flash the latest Tasmota firmware. Remember to set the Sonoff to programming mode by pressing down the button on Sonoff board while plugging in the FTDI USB to Serial board to the USB port.

   ![FTDI_connection](https://carbonpanda.github.io/images/FTDI_connection.jpg)

   ![flash_complete](https://carbonpanda.github.io/images/flash_complete.jpg)

2. Setup Sonoff-Tasmota WIFI SSID and password entries to connect to home network using Termite. Just type *SSID1 your_home_SSID* then *Password1 your_wifi_password*.

3. Allocate a fixed IP (personal preference) from home wireless router. Use wireless router DHCP IP reservation function to assign a fixed IP.

   ![Sonoff_IP](https://carbonpanda.github.io/images/Sonoff_IP.jpg)

4. Use a browser to enter the Sonoff Tasmota web interface with the assigned fixed IP. Or use Termite to setup the Sonoff such as MQTT host, MQTT topic and switch mode. Please refer to Tasmota Wiki on the [commands](https://github.com/arendst/Sonoff-Tasmota/wiki/Commands). 


Before setup the MQTT connection, it will be better to think or design how to structure the MQTT topics. If there are multiple rooms and devices to control, it might be better to define a hierarchy to identify the devices. For instance, master-room\main-light, guest-room\main-light, shower-room\water-heater, etc. However, if there is only one or two devices, then why bother.
