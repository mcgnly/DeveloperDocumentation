#  relayr for Web Developers

We don't want to teach you how to create a web app. We are quite positive that you know your way around web development tools. We've created an easy to use **JavaScript micro SDK** with the basic relayr related functionalities to get you started with your very first relayr Web Application.

Please note that this SDK is meant for web browser applications only. 

## Implementing the relayr JavaScript SDK

In order to start using the SDK all you would need to do is to include the `relayr-browser-sdk.min.js` file in your project:

	<script src="https://developer.relayr.io/relayr-browser-sdk.min.js"></script>


**Download the `relayr-browser-sdk.min.js` file from our [JavaScript micro SDK repository](https://github.com/relayr/browser-sdk)**

You could follow the guidelines for using the SDK on the repository's [README](https://github.com/relayr/browser-sdk/blob/master/README.md) or simply continue reading below.

----------

## Using the SDK

### Initializing the SDK 

The following function Initializes the Relayr SDK. It receives two parameters: 
`redirectURI` and `appID`

`Appid`: The ID of your app as it appears in the Developer Dashboard [API-Keys Section](https://developer.relayr.io/dashboard/apps/myApps).<br/>
`redirectUri`: The redirectUri provided on App creation. You may use *http://localhost* for locally-hosted projects. 

	var relayr = RELAYR.init({
	  appId: "YOUR_APP_ID",
	  redirectUri:"YOUR_REDIRECT_URL"
	});


----------


### Checking for the Existence of a Token

This function will check for the existence of a token (stored on your browser). If it does not find a token it will redirect to a Login page and use your `redirectUri` to come back with the token.

The function will recognize the token and automatically call the method `success`.

***success*** is a method called when the login flow is completed. 
(it has an optional token return variable: `success: function(token){}`)

	
	relayr.login({
	  success: function(token){
	  
	  }
	});


----------


### Retrieving a List of Devices

This function will return an array of the devices associated with your account.

**Note**: This function should be called following a successful Login.

	relayr.devices().getAllDevices(function(devices){
	        
	});


----------


### Subscribing to a Device Channel and Receiving Data

This function will subscribe the app to a device channel and start listening to your device. Data will be received into the `incomingData` method.
**Note**: This function needs to be called after a successful Login.

The function receives two variables:
 
`deviceId`: The ID of the device to subscribe to.

`incoingData`: This method is triggered each time the device sends data. 

`token`: (optional) you can pass a token variable you generated from the developer dashboard to subscribe to the device *without having to Log in*. 

	relayr.devices().getDeviceData({
	  deviceId: "YOUR DEVICE ID", 
	  incomingData: function(data){
	
	  }
	}); 


----------


### Retrieving User Properties         

This function returns an object with your user properties.
 
**Note**: This function should be called after a successful Login.

	relayr.user().getUserInfo();


----------
## Example: Receiving Data from your Sensors without logging in

We've made it possible for you to start viewing your sensors' data without having to provide an actual `redirectUri` and without having to log in to the platform.

In this scenario you will need to perform the initialization of the SDK, with the **AppID** of your app.

	  var relayr = RELAYR.init({
	    appId: "{YourAppId}"
	  });

And provide your **DeviceID** and **Token** in order to start receiving data. See instructions for generating your token [here](https://developer.relayr.io/documents/WebDev/OAuthToken). Your Device ID can be obtained from the [Devices section](https://developer.relayr.io/dashboard/devices), by clicking on the 'settings' icon on any of the devices. 
	
	  relayr.devices().getDeviceData({
	    deviceId: "{YourDeviceId}", 
	    token: token,
	    incomingData: function(data){
	      console.log("sensor",data);
	    }
	  });    


----------

Now that you understand how to incorporate receiving data via the relayr platform. You can go ahead and create your first web project. Check out our simple and quick <a href="https://github.com/relayr/cantTouchThis" target="_blank">Can't Touch This sample project</a> to get inspired.