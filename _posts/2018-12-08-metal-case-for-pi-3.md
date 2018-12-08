---
layout: post
title: "Fanless metal case for Raspberry Pi 3B and B+"
date: 2018-12-08
categories: [blog]
tags: [Raspberry Pi, metal housing, metal case, fanless]

---

# Fanless Pi 3B/B+ metal housing
My first Pi plastic housing fan has been getting dirty and noisy due to dust. 

![dirty fan](https://carbonpanda.github.io/images/dirty-fan.jpg)

So I was eager to throw it away and replace it with a true fanless housing. During the search, I learnt there is metal case for Pi 3B and I managed to find one on taobao.com. It is a perfect solution to me.

![metal case](https://carbonpanda.github.io/images/metal-case-close.jpg)

![metal case](https://carbonpanda.github.io/images/metal-case.jpg)

There are two variants available: one for 3B and one for 3B+ due to the different chip location. I end up having both variants.

## Pros and Cons
|      | Pros                          | Cons                                                         |
| ---- | :---------------------------- | :----------------------------------------------------------- |
| 1    | Fanless                       | Block wireless/BT signal                                     |
| 2    | Clean                         | More expensive than plastic case                             |
| 3    | Silent                        | Passive heat dissipation results in CPU warmer than using fan |
| 4    | Dissipate heat via metal body |                                                              |

As suggested by others people having the same case, I filed the metal columns with fine sand paper to remove the paint. 

![](https://carbonpanda.github.io/images/sanded-columns.jpg)

![](https://carbonpanda.github.io/images/sanded-contact.jpg)

Then I applied sufficient thermal paste between the contact points and screwed both upper and lower case together. 

![](https://carbonpanda.github.io/images/thermal-compound-at-columns.jpg)

![](https://carbonpanda.github.io/images/thermal-compound-at-contact.jpg)

![](https://carbonpanda.github.io/images/metal-case-bottom.jpg)

![](https://carbonpanda.github.io/images/pi-in-case.jpg)

That's it.

## Adding heatsinks
In order to get better heat dissipation, I decided to go overboard a bit and ordered two heatsinks from taobaon.com. 

![](https://carbonpanda.github.io/images/heatsink.jpg)

I attached them to the housing with wires (remember to remove the four rubber stubs below the case).

![](https://carbonpanda.github.io/images/with-heat-sink.jpg)

 I reckon I might get better result by sanding off the paint from the contact surfaces and using thermal paste but I didn't bother to do it. 

The additional heatsinks managed to lower the temperature by around 1 to 2°C. 

![](https://carbonpanda.github.io/images/CPU-temperature.jpg)

Note: NAS is hosted on a Pi 3B with metal case and same passive heatsinks running OpenMediaVault and Pi-hole DNS. HASSIO running without heatsinks attached from 9:00 am - 12:00 noon and with heatsink from 12:00 noon onwards. 

Obviously this is completely waste of money and time for such small amount of reduction. And I ordered two more sets for my other 3B and 3B+ anyway. ^_^;


## Measuring CPU temperature
Because the Home Assistant is running on this Pi box, I use a `command_line` platform to define a `sensor` for  the CPU temperature in sensors.yaml:

```
# hass.io CPU
  - platform: command_line
    name: 'HASSIO CPU Temperature'
    command: "cat /sys/class/thermal/thermal_zone0/temp"
    # If errors occur, remove degree symbol below
    unit_of_measurement: "°C"
    value_template: '{{ value | multiply(0.001) | round(1) }}'
    scan_interval: 600
```


Then I can get the temperature reading from entity `sensor.hassio_cpu_temperature`. Note: Space is being replaced by underscore on the name value.
