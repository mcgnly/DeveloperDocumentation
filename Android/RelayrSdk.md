# The RelayrSdk Class

The RelayrSdk Class serves as the access point to all endpoints in the Android SDK. It includes basic calls such as user login validation and can also call the handlers of the other classes- RelayrApi, RelayrBleSdk and WebSocketClient.

## Methods

### init(Context context) 

Initializes the SDK. Should be called when the {@link android.app.Application} is created. 

**Returned Value**: 

	 static void init(Context context);

### initInMockMode(Context context)

Initializes the SDK in Mock Mode. In this mode, mock reading values are generated. Used for testing purposes, without the need of a WunderBar or an internet connection. Should be called when the *{@link android.app.Application}* is created.

**Returned Value**: 

	 static void initInMockMode(Context context);

### getRelayrApi();

Returns the handler of the Relayr API. Used as an access point to the [RelayrApi](https://developer.relayr.io/documents/Android/RelayrApi) module.

**Returned Value**:

	  static RelayrApi getRelayrApi();

### logIn(Activity currentActivity, LoginEventListener listener)

Launches the login activity. Enables the user to log in to the relayr platform.

**Returned Value**:

	 static void logIn(Activity currentActivity, LoginEventListener listener);

### isUserLoggedIn()

Checks whether or not a user is logged in to the relayr platform. Returns true if the user is logged in, false otherwise.

**Returned Value**:

	 static boolean isUserLoggedIn(); 

### logOut()

Logs the user out of the relayr platform.

**Returned Value**:

	 static void logOut();

### getRelayrBleSdk()

Returns the handler of the Relayr BLE SDK. Used as an access point to the [RelayrBleSdk](https://developer.relayr.io/documents/Android/RelayrBleSdk) module.

**Returned Value**:

	 static RelayrBleSdk getRelayrBleSdk();

### isBleSupported()

Checks whether or not Bluetooth is supported. Should be called before calling the RelayrBleSdk handler *{@link #getRelayrBleSdk}*. Returns true if Bluetooth is supported, false otherwise.

**Returned Value**:

	 static boolean isBleSupported();

### promptUserToActivateBluetooth(Activity activity)

Prompts the user to activate Bluetooth. The method will not perform any action in case Bluetooth is not supported, i.e. if RelayrSdk.isBleSupported(); returns false.

**Returned Value**:

	static void promptUserToActivateBluetooth(Activity activity);

### isBleAvailable()

Checks whether Bluetooth is turned on or not. Returns true if Bluetooth is turned on, false otherwise. 
Should be called before calling the RelayrBleSdk handler *{@link #getRelayrBleSdk}*.The user can be prompted to activate their Bluetooth using *{@link #promptUserToActivateBluetooth}* 


**Returned Value**:	

	 static boolean isBleAvailable();
