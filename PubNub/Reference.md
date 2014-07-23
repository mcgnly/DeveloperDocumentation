# PubNub Integration - Overview

The relayr platform utilizes [PubNub](http://www.pubnub.com/) as its data distribution service. Once an application subscribes to a specific device channel (a sensor) it receives PubNub credentials which allows it to receive data from the specific device.
Communication with PubNub currently utilizes the Secure Web Sockets protocol. Since PubNub is an external service it essentially serves for data distribution only; The specific pairing keys, encryption keys and permissions for receiving data are maintained by the relayr platform and are generated and delivered during the subscription of the app to the device channel.

## Subscribing an App to a Device Channel

The API call which establishes the connection between a specific app and a device, and allows the app to start receiving data from it is the following:

####[POST] http://api.relayr.io/devices/{deviceId}/apps/{appId}

Where `appId` and `deviceId` are the IDs of the application and the device to be paired.

#### Implementation Example (JavaScript):

	
	    $.ajax({
		    url: "https://api.relayr.io/apps/"+appId+ "/devices/"+ deviceId,
		    type: 'post',
		    headers: {
		      "Authorization": 'Bearer '+ token 
		    },
		    dataType: 'application/json',
		    success: function (credentials) {
		 
		      var pubnub = PUBNUB.init({
		        ssl: true,
		        subscribe_key : credentials.pubSub,
		        cipher_key: credentials.cipherKey,
		        auth_key: credentials.authKey
		 
		      });


The call results in the delivery of the channel and the specific PubNub credentials for the channel (`cipherKey` and `authKey`).

#### For Example:

		{
		    "authKey": "8a292336-23d2-4f16-b7e1-1c17be11a532",
		    "cipherKey": "2d4589958e2f51ad6c590a6b0a4382c50c6713eef3d578210d2d135214b05eed",
		    "channel": "v1:6854bb1c-64ed-4d46-99b4-6cf28b72ad1d:data"
		}

## Example: Receiving Data from PubNub and Displaying it (JavaScript)

The example below is a simple step by step walk through the process of subscription to a device channel, data retrieval and display. 

**NOTE**: The code below relates to *private* devices, i.e. devices which have been registered under a specific user. The code below assumes that the credentials for the device have already been obtained. For additional information about device registration please see our [API](https://developer.relayr.io/documents/Registration/Devices).
To register your App please visit the [Apps section on the Developer Dashboard ](https://developer.relayr.io/dashboard/apps)


#### 1. Make sure to download the PubNub [SDK](http://www.pubnub.com/developers/) and incorporate it in your project.

#### 2. The example assumes the following parameters are known:

		var appId= "YOUR APP ID";
		var deviceId= "YOUR DEVICE ID";
		var token ="YOUR TOKEN";

#### 3. Initiate the API call which subscribes the App to the Device and returns PubNub credentials.

		
	    $.ajax({
		    url: "https://api.relayr.io/apps/"+appId+ "/devices/"+ deviceId,
		    type: 'post',
		    headers: {
		      "Authorization": 'Bearer '+ token 
		    },
		    dataType: 'application/json',
		    success: function (credentials) {
		 
		      var pubnub = PUBNUB.init({
		        ssl: true,
		        subscribe_key : credentials.pubSub,
		        cipher_key: credentials.cipherKey,
		        auth_key: credentials.authKey
		 
		      });

#### 4. Subscribe to the channel by passing the channel ID and output the data.

		 		pubnub.subscribe({
		        channel : credentials.channel,
		        
		        message : function(data){
		          //Output the realtime data coming from the device
		          document.write(data)
		        }
		      }); 
		    }
		});