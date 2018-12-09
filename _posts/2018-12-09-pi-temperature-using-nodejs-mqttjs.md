---
layout: post
title: "Using Node.js and MQTT.js to read and publish Pi CPU temperature"
date: 2018-12-09
categories: [blog]
tags: [Raspberry Pi, NAS, OMV, Pi-hole, temperature, Node.js, MQTT.js, Home Assistant]

---

# Reading and publishing Pi CPU temperature to Home Assistant using Node.js and MQTT.js

I have a Raspberry Pi 3B setup as a NAS and DNS server running [OpenMediaVault](https://www.openmediavault.org/) and [Pi-hole](https://pi-hole.net/) DNS.

A portable Western Digital 1T harddisk is attached via USB to the Pi box as the NAS storage. 

## NAS and DNS server

The portable harddisk is powered separately thru the USB port using a split USB cable. This is required as the Pi board doesn't have enough power to portable harddisk.

The Pi-hole DNS is a great tool to filter off advertisements. You can use it to bypass any annoying advertisements without using ad-blocker. In addition, you can monitor the Pi-hole statistics on Home Assistant.

Just define a Pi-hole platform in `sensors.yaml`:

```
# Pi Hole DNS
  - platform: pi_hole
    host: 192.168.86.40:8080
    monitored_conditions:
    - ads_blocked_today
    - ads_percentage_today
    - dns_queries_today
    - domains_being_blocked
    - queries_cached
    - queries_forwarded
    - unique_clients
    - unique_domains
```

And all pi-hole sensors can be grouped in groups.yaml:

```
dns:
  name: Pi Hole DNS Server
  icon: mdi:dns
  entities:
    - sensor.pihole_ads_blocked_today
    - sensor.pihole_ads_percentage_blocked_today
    - sensor.pihole_dns_queries_cached
    - sensor.pihole_dns_queries_forwarded
    - sensor.pihole_dns_queries_today
    - sensor.pihole_dns_unique_clients
    - sensor.pihole_dns_unique_domains
    - sensor.pihole_domains_blocked
```

![](https://carbonpanda.github.io/images/pi-hole-statistics.jpg)



## Reading and Publishing

To capture the Pi box CPU temperature, I followed [Pieter Beulque's article](http://weworkweplay.com/play/raspberry-pi-nodejs/) to use Node.js to capture the temperature and use MQTT to send the reading to Home Assistant. 

In addition, do following [MQTT.js GitHub page](https://github.com/mqttjs/MQTT.js/blob/master/README.md) to install MQTT.js module.

The following is the script `cpu_temp_mqtt.js` to read the CPU temperature reading and publish it via MQTT:

```
// define components
var fs = require("fs")
var mqtt = require('/usr/lib/node_modules/mqtt/mqtt.js')


// get first temperature reading
var temp = fs.readFileSync("/sys/class/thermal/thermal_zone0/temp")
var temp_c = temp/1000


// define timer object
let timer


// establish mqtt connection
var client  = mqtt.connect('mqtt://192.168.86.50',{
 	clientId: 'nas',
 	username: 'xxxxx',
 	password: 'yyyy'
})


// client event when mqtt connection established
client.on('connect', function () {

  	// subscribe to same topic to show published data
  	// mainly for debug purpose
  	client.subscribe('home/network/nas/cpu')
  
  	// publish cpu temperature value
  	client.publish('home/network/nas/cpu',temp_c.toString())

  	// setup reading interval in milliseconds.
  	// function loop will be called.
  	timer = setInterval(loop, 600000)
})


// client event when message received on topic
client.on('message', function (topic, message) {
  	// message is Buffer
  	// show received message
  	d = new Date()
  	n = d.toLocaleString()
	//  console.log(`${n}: Received message: ${message.toString()}`)

	// disconnect from mqtt server
	// mainly for debugging or development purpose
	//  client.end()
})


// loop event triggered by timer
function loop() {

  	// check mqtt connection
  	if (!client.connected) {
		// console.log('** mqtt connection closed')
    	// disable timer and end this script
    	return timer && clearInterval(timer)
  	}

  	// mqtt connection is still alive. read cpu temperature
  	temp = fs.readFileSync("/sys/class/thermal/thermal_zone0/temp")
  	temp_c = temp/1000

  	// publish temperature data
  	client.publish('home/network/nas/cpu',temp_c.toString())
	
	// disconnect from mqtt server
	// mainly for debugging or development purpose
	//  client.end()
}
```

The following line is added to `/etc/rc.local`:
```
node /opt/node/cpu_temp_mqtt.js &
```

So `cpu_temp_mqtt.js` will be triggered whenever Pi is rebooted.

A NAS CPU Temperature sensor is defined in `sensors.yaml` in Home Assistant:

```
# NAS CPU
  - platform: mqtt
    state_topic: 'home/network/nas/cpu'
    name: 'NAS CPU Temperature'
    unit_of_measurement: '°C'
    value_template: '{{ value | round(1) }}'
```

In Home Assistant, the temperature sensor entity can be grouped in `groups.yaml`:

```
network:
  name: Network Devices
  icon: mdi:wifi
  entities:
    - sensor.google_wifi_status
    - sensor.nas_cpu_temperature
    - sensor.hassio_cpu_temperature
```

![](https://carbonpanda.github.io/images/network-group.jpg)

