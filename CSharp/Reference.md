# relayr for C# Developers

We have some C# goodies just for you. We've created an easy to use [**C# SDK**](https://github.com/relayr/csharp-sdk) with the basic relayr related functionalities to get you started with your very first relayr C# Application. 

The SDK allows you to access your WunderBar sensor data and the [relayr Cloud Platform](https://developer.relayr.io/documents/Welcome/Platform) functionality from, you guessed it, apps built in C#!

Because it's built as a portable class library, you'll be able to use the same SDK across Windows 8.1, Windows Phone 8.1, and Windows Desktop Apps.

## Getting Started

The C# SDK has two main functions: 

1. Programatically exposing the relayr API. This is achieved through the HttpManager class.
2. Allowing applications to subscribe to the streams of data published by sensor modules and other connected devices. The Transmitter and Device classes handle this functionality. You will need security strings obtained through the HttpManager to subscribe to data topics.

### Getting an OAuth token

The HttpManager requires an OAuth token to authorize your HTTP requests. You can obtain a token in two manners:

1. Via an OAuth authentication flow: Due to the platform-specific nature of performing this task, the C# SDK does not have the ability to execute this process for you. If you choose to get a token using this method, you will need to implement it yourself. Documentation on how the relayr OAuth implementation can be found in [our documentation center](https://developer.relayr.io/documents/Welcome/OAuthReference).

2. By generating one through the relayr Dashboard. On the [API Keys](https://developer.relayr.io/dashboard/apps/myApps) page, create an app (or use an app previously created). 
Clicking the "Generate Token" button will create the required OAuth token. This value can be hard-coded into your app, and allows you access to data from sensors and devices associated with your relayr account. This is extremely useful for prototyping and personal-use apps, however, it prevents you from being able to make your app widely available. You will need to use the first method of generating the token, if you wish to distribute your application.

## Using the SDK

Subscribing to data published by a sensor is simple, and can be accomplished in just a few lines of code.

### Setting the Oauth Token

You are able to hard-code the generated OAuth token in the following manner:

        // Set the OAuth token for the requests initiated
        HttpManager.Manager.OauthToken = "YOUR OAUTH TOKEN HERE";

### Obtaining the User ID

This function returns the ID of the logged in user. It should be called after a successful login.

        // Obtain the userId
        HttpResponseMessage userInfoResponse = await HttpManager.Manager.PerformHttpOperation(
                                                    ApiCall.UserGetInfo, null, null);
        dynamic userInfo = await HttpManager.Manager.ConvertResponseContentToObject(userInfoResponse);
        string userId = (string) userInfo["id"];

### Retrieving a List of Transmitters associated with the User


        // Get a list of Transmitters
        HttpResponseMessage transmittersResponse = await HttpManager.Manager.PerformHttpOperation(
                                                        ApiCall.TransmittersListByUser,
                                                        new string[] { userId }, null);
        dynamic transmitters = await HttpManager.Manager.ConvertResponseContentToObject(transmittersResponse);

### Creating a Transmitter object for one of the transmitters retrieved 

In this case, we're using the first transmitter returned, although there could be many in the JSON response of the previous call.

        // Get the ID and Secret for the transmitter. These will be your credentials for the MQTT Broker
        string transmitterId = (string) transmitters[0]["id"];
        string transmitterSecret = (string) transmitters[0]["secret"];

### Connecting to the relayr MQTT Broker
        
        // Connect to the broker
        Transmitter transmitter = new Transmitter(transmitterId);
        transmitter.ConnectToBroker("SOME ID FOR YOURSELF", transmitterSecret);

### Subscribing to a Sensor's Data Stream

        // Subscribe to a sensor. Add an event handler for incoming data
        Device device = transmitter.SubscribeToDeviceData("SENSOR ID");
        device.PublishedDataReceived += device_PublishedDataReceived;

### Implementing an Event Handler for Newly Received Data

Decide how you would like to display the data and start viewing the values.

        // Event handler for incoming data
        void device_PublishedDataReceived(object sender, PublishedDataReceivedEventArgs args)
        {
            // Do something with the data here. Data is contained inside args.
        }

## Example: Sensor Channel Subscription Flow


        // Set the OAuth token for the requests initiated
        HttpManager.Manager.OauthToken = "YOUR OAUTH TOKEN HERE";
        
		// Obtain the userId
        HttpResponseMessage userInfoResponse = await HttpManager.Manager.PerformHttpOperation(
                                                    ApiCall.UserGetInfo, null, null);
        dynamic userInfo = await HttpManager.Manager.ConvertResponseContentToObject(userInfoResponse);
        string userId = (string) userInfo["id"];
        
		// Get a list of Transmitters
        HttpResponseMessage transmittersResponse = await HttpManager.Manager.PerformHttpOperation(
                                                        ApiCall.TransmittersListByUser,
                                                        new string[] { userId }, null);
        dynamic transmitters = await HttpManager.Manager.ConvertResponseContentToObject(transmittersResponse);


		// Get the ID and Secret for the transmitter. These will be your credentials for the MQTT Broker
        string transmitterId = (string) transmitters[0]["id"];
        string transmitterSecret = (string) transmitters[0]["secret"];

        // Connect to the broker
        Transmitter transmitter = new Transmitter(transmitterId);
        transmitter.ConnectToBroker("Identifier of your choice", transmitterSecret);

        // Subscribe to a sensor. Add an event handler for incoming data
        Device device = transmitter.SubscribeToDeviceData("SENSOR ID");
        device.PublishedDataReceived += device_PublishedDataReceived;

        // Event handler for incoming data
        void device_PublishedDataReceived(object sender, PublishedDataReceivedEventArgs args)
        {
            // Do something with the data here. Data is contained inside args.
        }
