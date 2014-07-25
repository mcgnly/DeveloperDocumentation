# The relayr Android SDK 

The relayr Android SDK provides Android app developers with a number of programmatic access points into the relayr API. API. Simpy follow the steps below to install the framework and integrate it into your project. 

##Setup
- Download the SDK from <a href="https://github.com/relayr/android-sdk/releases" target="_blank"> https://github.com/relayr/android-sdk/releases </a> 
- Extract the *android-sdk* or the tar.gz file.
- Import the Relayr SDK into Eclipse: File -> Import -> Existing Android code into workspace.
- Link the Relayr SDK to your project: Project -> Properties -> Android -> Library -> Add
 


#### Add the following to your Android Manifest:
		
		 
	  	< uses-permission android:name="android.permission.INTERNET" />
	  	< activity android:name="com.relayr.core.activity.Relayr_LoginActivity" />

## SDK Version

Returns the current version of the SDK

- **Endpoint:** Relayr_SDK.getVersion();
- **Returned value:** A string with the current version of the SDK.

**Example:**


	public void getVersion(View v) {
		try {
			Toast.makeText(MainActivity.this, Relayr_SDK.getVersion(), Toast.LENGTH_LONG).show();
		} catch (Relayr_Exception e) {
			e.printStackTrace();
		}
	}

	

	



