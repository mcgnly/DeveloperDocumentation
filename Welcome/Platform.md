#Introduction to the relayr Cloud Platform

##Overview

relayr provides you with a powerful platform which mediates between your applications and data gathered by sensor modules and other devices. It enables you to use the data regardless of the format and method in which it is gathered.
The platform receives data from the devices, stores it and distributes it only to the applications which incorporate the relayr SDK and meet the authorization criteria established when first registering on the platform.

As opposed to directly connecting an app  to the sensor modules over BLE (Bluetooth Low Energy), using the platform to connect to sensor modules and other devices can be achieved regardless of location and distance, it ensures that data is securely stored and is then available for further manipulation. 

Retrieval of historical data, which could be sliced according to any criteria desired and rule-based data retrieval are only a few of the options made available.

The relayr cloud platform also allows data normalization. Data is available in one format which the relayr SDK makes easy for processing, rendering the actual source of the data transparent. 

In case you are a device manufacturer you can also enjoy the simplicity enabled by the relayr platform. Simple integration with the platform introduces your devices to a world of developers who otherwise would not have had the ability to use your device readings.

##How is all this possible?
###Registration
The first stage that enables you to step into the relayr world is the registration. In this stage all basic entities such as the User, App, Device are introduced to the platform. The registration is done via the relayr Developer Dashboard. 

###OnBoarding 
The OnBoarding stage enables you to establish the pairing between your devices and your user credentials. the specific pairing credentials- authorization and encryption keys - are generated and maintained by the relayr routing component, mediated by the relayr API.  

###Flow
Below is a description of the basic data flow from the device (Sensor Module) to an App:


The Sensor Module (SM) delivers its readings over BLE to the Master Module (MM) which serves as a transmitter. The MM is the only module capable of communicating over WIFI. 

The MM delivers the data to the Device Integration Layer (DIL). The latter is essentially a Redis Cluster based PubSub server. The data is delivered over secure MQTT (MQTT/TLS). 

The data is distributed throughout the different cluster nodes and is either pushed to available subscribers or maintained in the cluster.
The relayr routing component, which  subscribes to the specific device channel during the onBoarding process receives the data from the DIL and encrypts it before sending it further.

The data is then delivered to PubNub over WSS (Secure Web Sockets) - an external service which serves as a distributor only. PubNub delivers the data received to the application subscribed to the specific device channel; a subscription which is also performed during the onBoarding process.


 