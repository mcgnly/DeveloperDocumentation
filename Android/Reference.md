# The relayr Android SDK 

The relayr Android SDK provides Android app developers with a number of programmatic access points into the relayr API. API. Simpy follow the steps below to install the framework and integrate it into your project. 

## A Few Words of Introduction

The relayr Android SDK consists of 4 main classes:

**The RelayrSdk Class** - This class is the backbone of the Android SDK, it includes basic authentication calls and serves as an access point to all other modules. 

**The RelayrApi Class** - This class incorporates a wrapped version of the relayr API calls.

**The RelayrBleSdk Class** - This class handles all calls which are related to BLE communication.

**The WebSocketClient Class** - This class incorporates all calls which are related to retrieval of sensor readings.

##Setup

####1. Create an Android Project 
See Thermometer demo for [instructions](https://github.com/relayr/android-demo-apps/commit/3e33f01c7e693e5ee0f9884dea8218731b8c9c8b)
####2. Create an empty Git repository or reinitialize an existing one:
	    
	    git init
	    
####3.  Add the android relayr sdk as a git submodule: 

Click [here](https://github.com/relayr/android-demo-apps/commit/f2c17c6a9a20f0c0e1f12cf8c38c2afd5ed4449d) to download  relayr Android Sdk
	    
	    git submodule add https://github.com/relayr/android-sdk.git libraries/relayr-sdk
	   
####4.  Reference the relayr SDK project in your settings.gradle file: 
	    
	    include ':app' ':libraries:relayr-sdk:android-sdk'
	    
####5.  Reference the relayr SDK project in your build.gradle inside your app folder:
	    
	    dependencies {
	        compile project(':libraries:relayr-sdk:android-sdk')
	    }
	    
####6.  Create a relayr [app](https://developer.relayr.io/dashboard/apps/myApps)
Download the application key once the app is created
 
####7.  Save the application key (relayrsdk.properties) file inside **src/main/assets** [see here](https://github.com/relayr/android-demo-apps/commit/06b85d467fdf6300367d6d997a0f89fc3b9a184c) 
 
####8. Subclass an Android Application inside your app and [initialize the Sdk](https://github.com/relayr/android-demo-apps/commit/27bef2e3c588c0e2351294a7fdc6418240af4bd4)
	    
	    public class MyApplication extends Application {
	        @Override
	        public void onCreate() {
	            super.onCreate();
	            RelayrSdk.initSdk(this);
	        }
	    }
	    
####9.  Reference your Application and add Internet permission in the Android Manifest
    
    <uses-permission android:name="android.permission.INTERNET" />
    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme"
        android:name=".MyApplication">
    
####10.  Log the user into the app. [See reference here](https://github.com/relayr/android-demo-apps/commit/19bf3578de9fd2c20e2ebab50c5a280500d411c9) 

  
		RelayrSdk.logIn(this, this);

####11. You are all set!
	    
    
##Examples

Take a look at our [Android Demos](https://github.com/relayr/android-demo-apps) to get started.

### SDK Version

Returns a string with the current version of the SDK

- **Endpoint:** RelayrSdk.getVersion();
- **Returned value:** A string with the current version of the SDK.

**Example:**

	 public static String getVersion();
	



	



