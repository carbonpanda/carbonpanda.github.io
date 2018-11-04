---
layout: post
title: "Adding a touch switch to Sonoff Basic"
date: 2018-11-04
categories: [blog]
tags: [sonoff, tasmota, touchswitch]

---

#Adding a touch switch

There are few reasons to add a touch switch to Sonoff:

1. It is cool, especially with LED backlit.
2. It can work as a latching switch so it is kinda stateless to the Sonoff relay state. For instance, after using a toggle switch to turn on or energize a Sonoff relay. Then you use Siri to turn off the Sonoff relay later, the toggle switch will still remain at the ON position. However, there is no such problem with touch switch in latched mode. It does not have ON or OFF position.

| Parts                                             | Qty  |                                      |
| ------------------------------------------------- | :-----: | :----------------------------------- |
| Heltec Automation Capacitance Touch Switch Module | 1    | ![](\images\heltec_touch_switch.jpg) |

Steps:

1. Solder two pins on the back of the touch switch to turn it to latched mode.

   ![touch_switch_in_latched_mode](\images\touch_switch_in_latched_mode.jpg)

2. Connect VCC, GND, and OUT pins from touch switch to Sonoff Basic 3V3, GND, GPIO14 pins respectively.

   ![touch_switch_to_sonoff_pins](\images\touch_switch_to_sonoff_pins.jpg)

   ![touch_switch_connected](\images\touch_switch_connected.jpg)

3. Power up Sonoff Basic via USB port using FTDI device and browse to Sonoff-Tasmota web interface. 

4. Navigate to **Configuration**, then **Configure Module**. 

5. Set Module Type to **01 Sonoff Basic** and GPIO14 Sensor to **09 Switch1**.

   ![sonoff_switch_mode_09](\images\sonoff_switch_mode_09.jpg)

6. Click Save.
