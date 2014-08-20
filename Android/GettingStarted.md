# Getting Started

Here are the first steps to get you started in the relayr world. Follow them and you'd be building your first app in no time!

#### Create a [relayr User](https://api.relayr.io/oauth2/auth?client_id=D-aSJGtuUeQPwIgos1Xt_xAhXzo9RpiR&redirect_uri=https://developer.relayr.io/dashboard/scrape&response_type=token&scope=access-own-user-info+configure-devices) in case you don't have one already

----------

####  Create a relayr app on the [app page](https://developer.relayr.io/dashboard/apps/myApps). 
Download the key file once the app is created
 

----------

#### Set up your environment for your first relayr Android project - See instructions [here](/AndroidEnvironment)


----------

####  Save the application key (relayrsdk.properties) inside *src/main/assets* 

See example [here](https://github.com/relayr/android-demo-apps/commit/06b85d467fdf6300367d6d997a0f89fc3b9a184c) 
 

----------

#### Subclass an 'Android Application' inside your app and [initialize the Sdk](https://github.com/relayr/android-demo-apps/commit/27bef2e3c588c0e2351294a7fdc6418240af4bd4)
	    
	    public class MyApplication extends Application {
	        @Override
	        public void onCreate() {
	            super.onCreate();
	            RelayrSdk.initSdk(this);
	        }
	    }

----------

	    
####  Reference your Application and add Internet permissions in the Android Manifest
    
    <uses-permission android:name="android.permission.INTERNET" />
    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme"
        android:name=".MyApplication">

----------

    
####  Log the user into the app 
  
		RelayrSdk.logIn(this, this);

Not quite sure how to do that? - See example [here](https://github.com/relayr/android-demo-apps/commit/19bf3578de9fd2c20e2ebab50c5a280500d411c9) 


----------

You are all set now to start building your relayr Android app! Have a look at our [SDK reference](/rendered-doc/javadoc) to learn more about our SDK	