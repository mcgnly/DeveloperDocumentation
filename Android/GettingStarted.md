# Getting Started

Here are the first steps to get you started in the relayr world. Follow them and you'd be building your first app in no time!

#### Create a [relayr User](https://api.relayr.io/oauth2/auth?client_id=D-aSJGtuUeQPwIgos1Xt_xAhXzo9RpiR&redirect_uri=https://developer.relayr.io/dashboard/scrape&response_type=token&scope=access-own-user-info+configure-devices) in case you don't have one already

----------

#### Set up your environment for your first relayr Android project - See instructions [here](https://developer.relayr.io/documents/Android/AndroidEnvironment)

----------

####  Create a relayr app on the [app page](https://developer.relayr.io/dashboard/apps/myApps). 
Download the key file once the app is created
 
----------

####  Place the application key (relayrsdk.properties) inside *src/main/assets* 

If the *assets* folder does not exist, create it in the above path.

----------

#### Create a java class inside *src/main/java/path/to/your/package* and extend the android.app.Application; Provide an onCreate method which initializes the SDK 
   
	    public class MyApplication extends Application {
	        @Override
	        public void onCreate() {
	            super.onCreate();
	            RelayrSdk.initSdk(this);
	        }
	    }

----------

	    
####  Inside *src/main/AndroidManifest.xml*: Reference your Application by adding the android:name to the application element and add Internet permissions as shown below
    
    <uses-permission android:name="android.permission.INTERNET" />
    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme"
        android:name=".MyApplication">

----------

    
####  Create a Login Sequence

In order for your app to allow users to see their devices, you would need to add a login sequence. Click [here](https://developer.relayr.io/documents/Android/LoginSequence) to learn more about how to create a login sequence.   
  
----------

You are all set now to start building your relayr Android app! Have a look at our <a href= "https://developer.relayr.io/rendered-doc/javadoc/index.html" target="_blank">SDK reference</a> to learn more about our SDK	