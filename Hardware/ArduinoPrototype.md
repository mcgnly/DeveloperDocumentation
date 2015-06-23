# The relayr - Arduino Repository 

We are proud to introduce the [relayr-Arduino library](https://github.com/relayr/relayr-arduino/blob/master/README.md)!
The code in this repository will enable you to use your Arduino to build a prototype of a device and connect it to the relayr platform, much like the one created with the [Particle.io Photon](https://github.com/relayr/relayr-photon). It contains instructions and a demo app which will make your first few steps in the relayr-arduino prototyping realm easy and fun!

## Adding your Arduino to the relayr cloud and receiving data-publishing credentials (Developer Dashboard)

You are now able to add your prototype directly from the Developer Dashboard. 

1. In the [My Devices](https://developer.relayr.io/dashboard/devices) page click the + sign on the bottom right part of the screen
	
2. Select 'Add Prototype'
3. Give your prototype a name and hit Start Prototyping
	
4. Enter the credentials received in the respective places in your code
	

## Adding your Arduino to the relayr cloud and receiving data-publishing credentials (curl)

1. Open a curl terminal window and enter the following to register your Arduino Yun:

    `curl -H "Content-Type: application/json" -H "Authorization: Bearer <your access token>" -X POST -d '{"name":"<your device name> ", "owner":"<your user id>", "integrationType":"wunderbar1"}' https://api.relayr.io/devices/`

2. Next, use the following to receive credentials for publishing data:

    `curl -H "Content-Type: application/json" -H "Authorization: Bearer <your access token>" -X POST -d '{"transport":"mqtt"}' https://api.relayr.io/devices/<your device id retrieved from the previous step>/transmitter`

The response received would look similar to this:

    {
    "id": "eabdb2ed-2375-4137-b006-da24c93c9507",
    "secret": "na1R6Hn3dYg1",
    "owner": "9210dce8-25da-4275-a73a-1383b9774255",
    "name": "no transmitter  no model device",
    "credentials": {
    "user": "eabdb2ed-2375-4137-b006-da24c93c9507",
    "password": "na1R6Hn3dYg1",
    "clientId": "T6r2y7SN1QTewBtokyTyVBw",
    "topic": "/v1/eabdb2ed-2375-4137-b006-da24c93c9507/"
    },
    "integrationType": "wunderbar1"
    }  

You should now be able to see your new device on the Developer Dashboard under [My Devices](https://developer.relayr.io/dashboard/devices)

You can also enter the credentials received into [this web tool](https://mqtt.relayr.io/) to see the data arriving. 

## 	Upgrading and your Arduino Yun

In the example created we'de used the Arduino Yun but it should also work with Arduino Ethernet, Arduino Ethernet Shield and Arduino WiFi Shield. 

If this is your first time using the Arduino Yun you will probably want to upgrade the openwrt-yun image. 
Download the openwrt-yun image [here](http://www.arduino.cc/en/Main/Software) and follow [this tutorial](http://www.arduino.cc/en/Tutorial/YunSysupgrade) to upgrade it.

If you don’t have a microsd card, you can try this [method](http://samjbrenner.com/notes/upgrading-openwrt-on-an-arduino-yun-without-a-microsd-card/) instead. 

### To upgrade the linux packages on the yun
1. Open a terminal and type `ssh root@arduino.local`
2. Type in your arduino’s password, default is ‘arduino’
3. Run the following command
		`opkg update`
4. Follow [this tutorial](http://www.arduino.cc/en/Guide/ArduinoYun) to configure the WiFi settings.

## Example

The example consists of an Arduino board connected to a moisture sensor and to a servo motor.

We use a simple web client to read the moisture measurements off the moisture sensor connected and to send a command which causes the servo motor to rotate.

The example code is using the Arduino YUN’s wifi client, if you’re using any of the other arduinos just remove the call to `Bridge.begin()` in the setup method and use the EthernetClient instead of the YunClient.

[This Pubsub library](https://github.com/knolleary/pubsubclient) is used for the MQTT connection and the [ArduinoJson](https://github.com/bblanchon/ArduinoJson) library is used for creating and parsing Json objects sent and received via MQTT

### The Prototype

In this example We used a moisture sensor as an input that publishes data in JSON format`{“meaning”:”moisture”,”value”:<value>}`.
The built in LED at pin 13 will blink each time the device its publishing.

We used a servo motor as an output which can be controlled via json commands over MQTT. The command format is the following 
`{“command”:”servo”,”value”:<value>}` 

This should rotate the motor clockwise/ counterclockwise according to the desired value with respect to the motor’s previous position.


### Configuring the Arduino Connections

1. After registering your Arduino on the relayr cloud (see the first step above) enter your credentials in the ***relayr-yun.ino*** file.  
2. Attach your own sensor to pin A5. (in the example we used a moisture sensor but any other sensor can be used).
3. Update your JSON in the publish method: 
  `dataToJson("moisture", analogRead(sensorPin));`
with your own sensor meaning. The first parameter is the meaning, the second is the value for your sensor
This will send your payload in the {"meaning":,"value":} format.

**The meaning parameter** denotes the meaning of the reading transferred. Meaning can be temperature, humidity, luminosity, color etc. 10. Flash your new firmware onto the photon.


### Configuring the Web Application

A simple html web app was built with angular and bootstrap to read and send commands from the arduino project using the [Javascript SDK](https://github.com/relayr/browser-sdk) 

To quickly configure and run it: 

1. Edit the javascript inside index.html with your own api key, you can your API key from the [API Key Section](https://developer.relayr.io/dashboard/apps/myApps) by creating a new app.
2. Place the key in the respective place in the code.
3. Go to the */www* folder, inside your cloned repository and run:
`python -m SimpleHTTPServer 8000` or `python3 -m http.server 8000` if you're using python 3.x
4. The example should now be available at: http://localhost:8000 and you should be able to see your new sensor data.
5. Click on the “Move 180” and “move 0” buttons to rotate the servo motor 180 degrees off and back to its original position respectively.


### Debugging 

You can use the Arduino’s serial monitor for debugging. You can also use Screen or PuTTy for this purpose.
Make sure you close your serial monitor *before* uploading new code onto the Arduino. As  Arduino uses one port to communicate with your laptop, you can’t use it to both upload your code and debug simultaneously.
You are also able to upload your code onto the YUN using its API.
