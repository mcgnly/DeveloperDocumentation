# Introduction
-----------------------------------------------------------------------------------------

Welcome to the relayr Java SDK. The relayr Java SDK provides you with programmatic access points to the relayr API, it allows you to use the relayr Cloud Platform to receive sensor data, manage devices, groups and other entities.

The SDK may be found [here](https://github.com/relayr/java-sdk).


## The *RelayrJavaSdk* Class

This static class serves as the access point to all endpoints in the Java SDK.
It includes the handlers of the other relevant API classes.

Before using any of the functionalities, the SDK needs to be initialized via the  RelayrJavaSdk.Builder, as shown below. 

<code>
new RelayrJavaSdk.Builder().setToken("Bearer your_token").build();
<code>

The token can be obtained manually from the Developer Dashboard. See instructions [here](https://github.com/relayr/java-sdk)

all API calls and other asynchronous calls in the SDK use RxJava.

## The *User* Class

The easiest way to start using the SKD is to obtain the *User* object.


    RelayrJavaSdk.getUser()
     		.subscribe(new Action1<User>() {
    @Override public void call(User user) {
       System.out.println(user.getName());
    }
    });


This is one of the main objects in Relayr SDK and it can be used to fetch user's devices, transmitters, groups etc.


user.getDevices()
	.subscribe(new Observer<List<Device>>() {
	@Override public void onCompleted() {}

	@Override public void onError(Throwable t) {}

	@Override public void onNext(List<Device> devices) {
	for (Device device : devices)
	System.out.println(device.getName());
	}
	});



	 user.getTransmitters()
	 	.subscribe(new Observer<List<Transmitter>>() {
	...
	@Override public void onNext(List<Transmitter> transmitters) {
	for (Transmitter transmitter : transmitters)
	System.out.println(transmitter.getName());
	}
	});



	 user.getGroups().subscribe(new Observer<List<Group>>() {
	        ...
	        @Override public void onNext(List<Group> groups) {
	            for (Group group : groups)
	                System.out.println(group.getName());
	        }
	    });


Every mentioned object is wrapped in the SDK so direct usage of APIs is not necessary. However it is still possible to obtain any of the desired API handlers through  the appropriate method and perform direct [API calls](http://docs.wunderbarregistration.apiary.io/).


	RelayrJavaSdk.getUserApi().getUserInfo().subscribe(new Observer<User>() {
        ...
        @Override public void onNext(User user) { }
    });


## The *Device* Class

The device entity is any external entity capable of gathering measurements or one which is capable of receiving information from the relayr platform (examples would be a thermometer, a gyroscope or an infrared sensor)

Devices can be obtained in different ways. You can get all of your devices:


	user.getDevices().subscribe(new Observer<List<Device>>() {
		...
	    @Override public void onNext(List<Device> devices) { 
	    	//do something my devices
	    }
	});


You can get your transmitters (e.g Wunderbars) and then get devices under a certain transmitter:


	user.getTransmitters()
    .flatMap(new Func1<List<Transmitter>, Observable<List<TransmitterDevice>>>(){
        @Override
        public Observable<List<TransmitterDevice>> call(List<Transmitter> transmitters) {
            return transmitters.get(0).getDevices();
        }
    })
    .subscribe(new Observer<List<TransmitterDevice>>() {
        ...
        @Override public void onNext(List<TransmitterDevice> devices) {
           //do something my devices
        }
    });


You can also get your devices under a group:


 	user.getGroups()
    .map(new Func1<List<Group>, List<Device>>() {
        @Override public List<Device> call(List<Group> groups) {
            return groups.get(0).getDevices();
        }
    })
    .subscribe(new Observer<List<Device>>() {
        ...
        @Override public void onNext(List<Device> devices) {
			//do something my devices
        }
    });


When you have obtained the device you can get data from it in the following manner:


	device.subscribeToCloudReadings()
        .subscribe(new Observer<Reading>() {
            @Override public void onCompleted() {}

            @Override public void onError(Throwable e) {}

            @Override
            public void onNext(Reading reading) {
                System.out.printf("Value " + reading.value);
            }
        });


Though getting the data from your devices may be easy, there is little use for it if you are unable to parse it. Every device has a *modelId* which can be used to obtain another entity called DeviceModel and so make sense of the data received.

## The *DeviceModel* Class

This is the main object that specifies parsing details (JSON schema) for every device supported on relayr platform.
*DeviceModel* is based on the device firmware and for each firmware version we defined one or more transport options (BLE, cloud...). Transport defines readings, commands and configurations to interact with the device. 

For example let's take a look at how the Wunderbar Temperature and Humidity sensor is defined (We'll focus only on the readings):


	{
	  "id": "ecf6cf94-cb07-43ac-a85e-dccf26b48c86",
	  "name": "Wunderbar Thermometer & Humidity Sensor",
	  ...
	  "firmware": {
	    "1.0.0": {
	      "transport": {
	        "cloud": {
	          "commands": [],
	          "readings": [
	            {
	              "path": "",
	              "meaning": "temperature",
	              "valueSchema": {
	                "type": "number",
	                "unit": "celsius",
	                "maximum": 100,
	                "minimum": -100
	              }
	            },
	            {
	              "path": "",
	              "meaning": "humidity",
	              "valueSchema": {
	                "type": "number",
	                "unit": "percent"
	              }
	            }
	          ],
	          "configurations": []
	        }
	      },
	     ...
	    }
	  }
	}


We can see that we have two types of readings, temperature and humidity, they have a field called "meaning" and a "*valuSchema*" field that defines the data type and other related fields.

Let's see how to use this in the code. First we can extract all the readings for the Wunderbar temperature sensor:

 
	final Map<String, DeviceReading> readings = new HashMap<>();
	try {
	    DeviceModel model = RelayrJavaSdk.getDeviceModelsCache().getModelByName("Wunderbar Thermometer", false);
	    DeviceFirmware firmware = model.getLatestFirmware();
	    Transport transport = firmware.getDefaultTransport();
	
	    for (DeviceReading reading : transport.getReadings())
	        readings.put(reading.getMeaning(), reading);
	} catch (DeviceModelsException e) {
	    e.printStackTrace();
	}


Now when we have *DeviceReading* specifications we can use the 'meaning' field to filter the Reading objects arriving from device:


 	device.subscribeToCloudReadings()
    .subscribe(new Observer<Reading>() {
        @Override public void onCompleted() {}

        @Override public void onError(Throwable e) { }

        @Override
        public void onNext(Reading reading) {
            DeviceReading deviceReading = readings.get(reading.meaning);

            if (deviceReading.getValueSchema().getSchemaType() == SchemaType.NUMBER){
                System.out.printf("Meaning " + reading.meaning);
                System.out.printf("Value " + ((Double) reading.value).intValue());
            }
        }
    });


This parsing of a simple double value from the temperature sensor may seem like an over complication but it will serve helpful with more complicated devices.
It will tell you which possible commands and configurations you can send to your device.


A simple example of a use case for the SDK may be found [here](https://github.com/relayr/java-sdk-example)