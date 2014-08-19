# The relayr Android SDK 

The relayr Android SDK provides Android app developers with a number of programmatic access points into the relayr API. Simpy follow the steps below to install the framework and integrate it into your project. 

## A Few Words of Introduction

The relayr Android SDK consists of 4 main classes:

**The RelayrSdk Class** - This class is the backbone of the Android SDK, it includes basic authentication calls and serves as an access point to all other modules. 

**The RelayrApi Class** - This class incorporates a wrapped version of the relayr API calls.

**The RelayrBleSdk Class** - This class handles all calls which are related to BLE communication.

**The WebSocketClient Class** - This class incorporates all calls which are related to retrieval of sensor readings.

----------

##Setup

#### Create an Android Project 
See the Android [Thermometer demo](https://github.com/relayr/android-demo-apps/commit/3e33f01c7e693e5ee0f9884dea8218731b8c9c8b) for instructions. 



#### Create an empty Git repository or re-initialize an existing one:
	    
	    git init
	    
#### Click [here](https://developer.relayr.io/dashboard/sdk) to download the relayr Android SDK.

####  Add the android relayr SDK as a git sub-module: 
  
	    git submodule add https://github.com/relayr/android-sdk.git libraries/relayr-sdk

See example [here](https://github.com/relayr/android-demo-apps/commit/f2c17c6a9a20f0c0e1f12cf8c38c2afd5ed4449d) 
	   
####  Reference the relayr SDK project in your settings.gradle file: 
	    
	    include ':app' ':libraries:relayr-sdk:android-sdk'
	    
####  Reference the relayr SDK project in the build.gradle file inside your app folder:
	    
	    dependencies {
	        compile project(':libraries:relayr-sdk:android-sdk')
	    }
	    
####  Create a relayr app on the [Developer Dashboard](https://developer.relayr.io/dashboard/apps/myApps). 
Download the application (the relayrsdk.properties file)  once the app is created
 
####  Save the application key (relayrsdk.properties) inside *src/main/assets* 

See reference [here](https://github.com/relayr/android-demo-apps/commit/06b85d467fdf6300367d6d997a0f89fc3b9a184c) 
 
#### Subclass an 'Android Application' inside your app and [initialize the Sdk](https://github.com/relayr/android-demo-apps/commit/27bef2e3c588c0e2351294a7fdc6418240af4bd4)
	    
	    public class MyApplication extends Application {
	        @Override
	        public void onCreate() {
	            super.onCreate();
	            RelayrSdk.initSdk(this);
	        }
	    }
	    
####  Reference your Application and add Internet permissions in the Android Manifest
    
    <uses-permission android:name="android.permission.INTERNET" />
    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme"
        android:name=".MyApplication">
    
####  Log the user into the app 
  
		RelayrSdk.logIn(this, this);

[See reference here](https://github.com/relayr/android-demo-apps/commit/19bf3578de9fd2c20e2ebab50c5a280500d411c9) 

#### You are all set!
	    
----------------------------------------
   
### SDK Version

Returns a string with the current version of the SDK

- **Endpoint:** RelayrSdk.getVersion();
- **Returned value:** A string with the current version of the SDK.

**Example:**

	 public static String getVersion();
	
-------------------------------------------------------------

## SDK Methods 

For a full list of the relayr SDK methods click [here]() 

## Sample Projects

Click [here](https://github.com/relayr/android-demo-apps) to get inspired	



