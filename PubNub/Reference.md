# PubNub Integration - Overview

The relayr platform utilizes [PubNub](http://www.pubnub.com/) as its data distribution service. Once an application subscribes to a specific device channel (a sensor) it receives PubNub credentials which allows it to receive data from the specific device.
Communication with PubNub currently utilizes the Secure Web Sockets protocol. Since PubNub is an external service it essentially serves for data distribution only; The specific pairing keys, encryption keys and permissions for receiving data are maintained by the relayr platform and are generated and delivered during the subscription of the app to the device channel.

## Subscribing an App to a Device Channel

The API call which establishes the connection between a specific app and a device, and allows the app to start receiving data from it is the following:

####[POST] http://api.relayr.io/devices/{deviceId}/apps/{appId}

Where `appId` and `deviceId` are the IDs of the application and the device to be paired.

#### Implementation Example (JavaScript):

	
	    $.post("https://api.relayr.io/devices/"+ device.id +"/subscription", function(credentials){
	
	      //create a new instance of pubNub and pass the credentials to the function
	      var pubnubClient = new PubnubClient(credentials, device.id);
	    });
	  	}


The call results in the delivery of the channel and the specific PubNub credentials for the channel (`cipherKey` and `authKey`).

#### For Example:

		{
		    "authKey": "8a292336-23d2-4f16-b7e1-1c17be11a532",
		    "cipherKey": "2d4589958e2f51ad6c590a6b0a4382c50c6713eef3d578210d2d135214b05eed",
		    "channel": "v1:6854bb1c-64ed-4d46-99b4-6cf28b72ad1d:data"
		}

## Example: Receiving Data from PubNub and Displaying it (JavaScript) 

#### 1. Make sure to download the PubNub [SDK](http://www.pubnub.com/developers/) and incorporate it in your project.

#### 2. Start by getting a list of the available public devices on the relayr platform. In case you have a registered device, you may skip this stage

		//Main Call
	  		
			function getPublicDevices(){
	
	    //You can use Jquery GET to retrieve a list of public devices from the relayr API
	    $.get("https://api.relayr.io/devices/public", function(publicDevices){
	     
	      //Iterate through the first 5 devices found
	      publicDevices.slice(0, 5).forEach(function(device){
	        //Get the credentials for
	        subscribeDevice(device);
	
	        //Visualization Option: create a container for each device
	        $("body").append("<div id='container' device-id='"+device.id+"'><h3>Device: "+device.id+"</h3></div>");
	      });
	    });
	  	} 

#### 3. Initiate the call which returns PubNub Credentials from the relayr API. 

		//Returns credentials for a device
			function subscribeDevice(device){
	
	    
	    //POST to the API with the device id in the url to receive the credentials for Pubnub
	    	$.post("https://api.relayr.io/devices/"+ device.id +"/subscription", function(credentials){
	
	      //create a new instance of pubNub and pass the credentials to the function
		      var pubnubClient = new PubnubClient(credentials, device.id);
		    });
		  }
	
	
	  	//It can also be an instance for a number of devices
	 	 function PubnubClient(credentials, id){
	    
#### 4. Initialize the PubNub SDK *init* using the relayr credentials recived
	    var pubnub = PUBNUB.init({
	      ssl: true,
	      subscribe_key : credentials.pubSub,
	      cipher_key: credentials.cipherKey,
	      auth_key: credentials.authKey
	
	    });

#### 5. Subscribe to the device's channel by passing the channel ID

		pubnub.subscribe({
      	channel : credentials.channel,
      
      	message : function(data){

        //Visualization Option: Print the incoming data
        $("div#container[device-id='"+id+"']").append("<p>"+ data+"</p>");

      	} 


