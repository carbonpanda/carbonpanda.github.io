```
layout: post
title: "Replacing water heater switch with Sonoff Basic"
date: 2018-12-01
categories: [blog]
tags: [sonoff, tasmota, touchswitch]
```



# Using Sonoff Basic to control water heater



The water heater is located at guest bathroom. And it is very troublesome to turn it on or off when I'm in the master room. In addition, it is annoying when you found out it is turned off by someone when you are taking a shower. Will it be nice if you can just ask Google Assistant or Siri to turn on the water heater for you? So this is the motivation for me to replace the switch.

The water heater has a max. load of 7.5A. So it is within the 10A limit of Sonoff Basic. The build is simple. The only troublesome part is replacing the wall switch box and the wiring.

The existing wall switch consists of two 1 gang box.

![old wall switch](https://carbonpanda.github.io/images/old-wall-switch-box.jpeg)

And there is a 220V small light bulb as the indicator light.

![water heater switch plate](https://carbonpanda.github.io/images/old-switch-front-plate.jpg)

Since there is not sufficient room to house the Sonoff in the 1 gang back box, I need to get a 2 gang back box to house the Sonoff PCB. The Sonoff case is just too big. 

| Parts                   | Qty  |                                                              |
| ----------------------- | :--: | :----------------------------------------------------------- |
| 2-gang back box         |  1   | ![gang box](https://carbonpanda.github.io/images/2_gang_back_box.jpg) |
| Blank front plate       |  1   | ![Blank plate](https://carbonpanda.github.io/images/blank-plate.jpg) |
| New water heater switch |  1   | Just to reuse the small light bulb.                          |
| 8A Fuse and fuse holder |  1   | ![Fuse](https://carbonpanda.github.io/images/fuse.jpg)       |

## Prepare the Sonoff switch

Steps:

1. Setup Home Assistant configuration.yaml file with following section:

![HA MQTT](https://carbonpanda.github.io/images/ha-configuration-mqtt.JPG)

![HA Switch](https://carbonpanda.github.io/images/ha-configuration-switch.JPG)

2. Browse to Sonoff web console to setup WIFI and MQTT

3. Take the small light bulb from the new water heater switch and connect it to the 22V output on Sonoff

4. Connect the Sonoff to a 220V source to power up the system.

5. Time to test out the Sonoff. I put the whole assembly on a ceramic bowl for safety. 

   ![Sonoff with touch switch](https://carbonpanda.github.io/images/sonoff-touch-assembly.jpg)

The Sonoff shall be able to turn on the light bulb by pressing the little push button, or by touching the touch switch. In addition, the Home Assistant shall also able to control it by calling service switch.turn_on.

![calling switch.turn_on service](https://carbonpanda.github.io/images/ha-call-switch-service.jpg)



## Replace the wall switch box

Steps:

1. Remove the old switch 

2. Replace the old 1-gang back boxes

3. Put back the other switch front plate

4. Drill a hole on the blank switch front plate for the little light bulb.

5. Hot glue the light bulb and touch switch to the new blank switch front plate.

   ![attach touch switch](https://carbonpanda.github.io/images/hot-glue-light-touch-switch.jpg)

6. Put a in-line fuse, 8A - 10A, between the main 220V source and the Sonoff. This is to safeguard the Sonoff.

7. Connect the water heater to the Sonoff  in parallel with the little light bulb indicator.

8. Hot glue the Sonoff PCB board in the 2-gang back box.

   ![attach PCB](https://carbonpanda.github.io/images/hot-glue-sonoff-pcb.jpg)

9. Make a paper cone, use duck tape, or whatever you fancy to cover the green Sonoff LED if you don't want to see the green status light.  

10. Put back the blank switch front plate to the back box to finish the job.

    ![done](https://carbonpanda.github.io/images/completed-water-heater-switch.jpg)

11. The Sonoff Basic shall able to control the water heater via the touch switch.
    <video width="320" height="240" controls>
      <source src="https://carbonpanda.github.io/images/sonoff-water-heater-switch.mp4" type="video/mp4">
    </video>




