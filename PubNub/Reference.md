# Pubnub usage on the relayr platform 

The relayr platform utilizes <a href="(http://www.pubnub.com/" target="_blank"> PubNub </a>  as its data distribution service. We use PubNub as our distributor because of the flexibility it allows. It can publish data to any subscriber - be it a server a device or an app. The method of publishing varies according to the quality of the network connection.   

Once an application subscribes to a specific device channel (a sensor) it receives PubNub credentials which allows it to receive data from the specific device.
Communication with PubNub currently utilizes the Secure Web Sockets protocol. Since PubNub is an external service it essentially serves for data distribution only; The specific pairing keys, encryption keys and permissions for receiving data are maintained by the relayr platform and are generated and delivered during the subscription of the app to the device channel.

The API calls which handle the communication with PubNub are integrated in the [Android SDK](https://developer.relayr.io/documents/Android/Reference) and [iOS Framework](https://developer.relayr.io/documents/iOS/Reference), you can have a look at a simple JavaScript implementation in our WEB application section [here](s)

