
# Subscribing to a Public Device and Receiving Data

The example below is a simple step by step walk through the process of subscription to a public device channel, data retrieval and display. 

**NOTE**: The API calls referenced below are integrated in the [Android SDK](https://developer.relayr.io/documents/Android/Reference) and [iOS Framework](https://developer.relayr.io/documents/iOS/Reference), the below example is a suggestion for implementation in a JavaScript based client.

**For a complete code sample of the above process for *public* devices please see <a href="https://gist.github.com/SmbatYeranyan/a2861a59248aa87a1187" target="_blank"> this project on GitHub </a>** 

#### 1. Make sure to download the PubNub <a href="http://www.pubnub.com/developers/" target="_blank"> SDK </a> and incorporate it in your project.

#### 2. Start by getting a list of the available public devices on the relayr platform. 

		//Main Call
	  		
		function getPublicDevices(){
	
		    //You can use Jquery GET to retrieve a list of public devices from the relayr API
		    $.get("https://api.relayr.io/devices/public", function(publicDevices){
		     
		      //Iterate through the first 5 devices found
		      publicDevices.slice(0, 5).forEach(function(device){
		        //Get the credentials for
		        subscribeDevice(device);
		
		      });
		    });
	  	} 

#### 3. Initiate the call which returns PubNub Credentials from the relayr API. 

			//Returns credentials for a device
			function subscribeDevice(device){
	
	    
	    	//POST to the API with the device id in the url to receive the credentials for Pubnub
	    	$.post("https://api.relayr.io/devices/"+ device.id +"/subscription", function(credentials){
	      		//create a new instance of pubNub and pass the credentials to the function
		      	var pubnubClient = new PubnubClient(credentials);
		    });
		  }
	
	
	    
#### 4. Initialize the PubNub SDK *init* using the relayr credentials received.
	    
		//It can also be an instance for a number of devices
	 	 function PubnubClient(credentials){		
			var pubnub = PUBNUB.init({
		      ssl: true,
		      subscribe_key : credentials.pubSub,
		      cipher_key: credentials.cipherKey,
		      auth_key: credentials.authKey
		
		 	});

#### 5. Subscribe to the device's channel by passing the channel ID and display the data received.
	
		pubnub.subscribe({
      		channel : credentials.channel,
      
      		message : function(data){

			document.write(data)

  		});
	}

