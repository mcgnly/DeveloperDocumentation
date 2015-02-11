# relayr for C# and .NET Developers

We have some C# goodies just for you. We've created an easy to use [**C# SDK**](https://github.com/relayr/csharp-sdk) with the basic relayr related functionalities to get you started with your very first relayr C# Application. 

The SDK allows you to access your WunderBar sensor data and the [relayr Cloud Platform](https://developer.relayr.io/documents/Welcome/Platform) functionality from, you guessed it, apps built in C#!

Because it's built as a portable class library, you'll be able to use the same SDK across Windows 8.1, Windows Phone 8.1, and Windows Desktop Apps.

## Getting Started

The C# SDK has two main functions: 

1. Programatically exposing the relayr API. This is achieved through the HttpManager class.
2. Allowing applications to subscribe to the streams of data published by sensor modules and other connected devices. The Transmitter and Device classes handle this functionality. You will need security strings obtained through the HttpManager to subscribe to data topics.

To include the C# library simply download it from our [C# Github repository ](https://github.com/relayr/csharp-sdk) and add it as a project to your own C# project.

### Importing NuGet Libraries

Before you can build a project including the C# SDK, you'll need to import two libraries via the [NuGet](https://www.nuget.org/) package manager in Visual Studio.


1. In Visual Studio, go to *Tools > NuGet Package Manager > Manage NuGet Packages for Solution.*

2. Select *Online* in the left hand menu and then search for and install the following two packages:

	**Json.NET** and **M2Mqtt**

### Getting an OAuth token

The HttpManager requires an OAuth token to authorize your HTTP requests. You can obtain a token in two manners:

Via an OAuth authentication flow: Due to the platform-specific nature of performing this task, the C# SDK does not have the ability to execute this process for you. If you choose to get a token using this method, you will need to implement it yourself. Documentation on how the relayr OAuth implementation can be found on our [OAuth Reference page](https://developer.relayr.io/documents/Welcome/OAuthReference).

or

By generating one through the relayr Dashboard. On the [API Keys](https://developer.relayr.io/dashboard/apps/myApps) page, create an app (or use an app previously created). 
Clicking the "Generate Token" button will create the required OAuth token. You can find detailed instructions for this procedure [here](https://developer.relayr.io/documents/WebDev/OAuthToken). 

This value can be hard-coded in your app, and allows you access to data from sensors and devices associated with your relayr account. This is extremely useful for prototyping and personal-use apps, however, it prevents you from being able to make your app widely available. You will need to use the first method of generating the token, if you wish to distribute your application.

## Using the SDK

Below you will find the basic functions of the SDK. These will allow you to fetch a user, fetch the transmitter entities associated with the user as well as subscribing to data arriving from the different sensors.

### Setting the OAuth Token
Once you've obtained your OAuth token using either of the methods described above, pass it to the relayr class in the following manner:

		// Set the OAuth token
		Relayr.OauthToken = "YOUR OAUTH TOKEN HERE";

### Retrieving Transmitters and Connecting to the MQTT Broker

Next, you'll need to fetch a list of your transmitters (WunderBars) from the relayr API, using the `GetTransmitters()` function. 
You will use the ID of the transmitter to connect to the MQTT broker. Connections to the broker are performed on a per-Transmitter basis. You can have multiple connections simultaneously, one for each WunderBar. If you have multiple transmitters (Multiple WunderBars) you can choose an Id for each in order to tell them apart. This Id will be passes as `CLIENT ID OF YOUR CHOICE` below.

		// Get a list of transmitters and connect to the MQTT broker with the first one in the list
		List<dynamic> transmitters = await Relayr.GetTransmittersAsync();
		Transmitter transmitter = Relayr.ConnectToBroker(transmitters[0], "CLIENT ID OF YOUR CHOICE");

### Retrieving a List of Sensors and Subscribing to Data

Finally, fetch a list of Devices (sensors) associated with a specific Transmitter by calling the `GetDevices()` function, in the Transmitter object instance. 
Once you have a sensor, you'll subscribe to the data being published by that sensor and define an event handler where you can choose how to handle the data received.

In the example below, we've subscribed to the first device on the list of associated devices.

		// Get a list of devices associated with the transmitter, subscribe to data
		// From the first device on the list
		List<dynamic> devices = await transmitter.GetDevicesAsync();
		Device device = await transmitter.SubscribeToDeviceDataAsync(devices[0]);
		device.PublishedDataReceived += device_PublishedDataReceived;

	// Create Handler for the the sensor's data published event
	void device_PublishedDataReceived(object sender, PublishedDataReceivedEventArgs args)
	{
	      // Do something with the data here. Data is contained inside args
	}

## Example: Sensor Channel Subscription Flow

The example below is a typical use of the SDK, allowing you to subscribe to data arriving from a sensor.

        
		// Set the OAuth token
		Relayr.OauthToken = "YOUR OAUTH TOKEN HERE";
		
		// Get a list of transmitters and connect to the MQTT broker with the first one in the list
		List<dynamic> transmitters = await Relayr.GetTransmittersAsync();
		Transmitter transmitter = Relayr.ConnectToBroker(transmitters[0], "CLIENT ID OF YOUR CHOICE");
	
		// Get a list of devices associated with the transmitter, subscribe to data
		// From the first device on the list
		List<dynamic> devices = await transmitter.GetDevicesAsync();
		Device device = await transmitter.SubscribeToDeviceDataAsync(devices[0]);
		device.PublishedDataReceived += device_PublishedDataReceived;

	// Create Handler for the the sensor's data published event
	void device_PublishedDataReceived(object sender, PublishedDataReceivedEventArgs args)
	{
	      // Do something with the data here. Data is contained inside args
	}