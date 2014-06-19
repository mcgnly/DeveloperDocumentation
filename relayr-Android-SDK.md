# The relayr Android SDK 

The relayr Android SDK provides Android app developers with a number of programmatic access points into the relayr API

##Setup
- Download the SDK from https://github.com/relayr/android-sdk/releases
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
	
# Users

The following endpoints concern the _User_ entity. This entity is the most basic one, on the relayr platform and allows the addition of other entities.

### User Login

Prompts the user with the relayr login dialog.

- **Endpoint:** Relayr_User.login(); 
- **Returned value:** None

### Check User Login

Checks whether a user is logged in or not. 

- **Endpoint:** Relayr_SDK.isUserLogged(); 
- **Returned value:**  Boolean: TRUE in case a user is logged in, FALSE otherwise. 


### Logout a User

Performs a user logout.

- **Endpoint:** Relayr_SDK.logout(); 
- **Returned value:** None

###  Login / Logout Events Notifications

You may define the relayr Events listener in order to recieve notifications about successful or insucceessful login and logout events.

**Example**
		
		Relayr_SDK.setLoginEventListener(new LoginEventListener() {
			public void onUserLoggedInSuccessfully() {
				Log.d("MainActivity", "User logged in");
			}

			public void onUserLoggedOutSuccessfully() {
				Log.d("MainActivity", "User logged out");
			}

		});

### Get User Info

Returns information about the current logged in user.

- **Endpoint:** getCurrentUserName();
- **Returned Value:** A relaye_User object with the following attributes: `Id`, `firstName`, `lastName`, `email`


**Example**
		
		public static void getCurrentUserName() throws Relayr_Exception {
			new AsyncTask<Void, Void, Void>() {
				@Override
				protected Void doInBackground(Void... params) {
					try {
						Object[] parameters = {};
						Relayr_User user;
						user = Relayr_ApiConnector.doCall(Relayr_ApiCall.UserInfo, parameters);
						Log.d(“Test”, “Username: ” + user.getFirstName());
					} catch (Relayr_Exception e) {
						Log.d("Test", "Error: " + e.getMessage());
					}
					return null;
				}
			}.execute();
		}

### Update User Info

Updates the information of the current logged in user.

- **Endpoint:**  Relayr_SDK.updateUserInfo(newInfo); The attributes that can be modified are `Id`, `firstName`, `lastName`, `email`
- **Returned Value:** A relaye_User object  
 
**Example**

		public void updateUserInfo(View v) {
			new AsyncTask<Void, Void, Void>() {
				@Override
				protected Void doInBackground(Void... params) {
					try {
						HashMap<String, Object> newInfo = new HashMap<String, Object>();
						newInfo.put("firstName", "Android SDK");
						Relayr_User user = Relayr_SDK.updateUserInfo(newInfo);
						StringBuilder stringBuilder = new StringBuilder();
						stringBuilder.append("[\n");
						stringBuilder.append(user.toString() + ",\n");
						stringBuilder.append("]");
						openAlert(stringBuilder.toString());
					} catch (Relayr_Exception e) {
						openAlert("Error: " + e.getMessage());
					}
	
					return null;
				}
			}.execute();
		}


### List All Devices under a Specific User

Returns a list of all devices registered under a user. 

- **Endpoint:**  Relayr_SDK.getUserDevices();  
- **Returned value:** An Array of relayr_User objects

**Example**

		public void listUserDevices(View v) {
				new AsyncTask<Void, Void, Void>() {
					@Override
					protected Void doInBackground(Void... params) {
						try {
							ArrayList<Relayr_Device> devices = Relayr_SDK.getUserDevices();
							StringBuilder stringBuilder = new StringBuilder();
							stringBuilder.append("[\n");
							for (Relayr_Device device:devices) {
								stringBuilder.append(device.toString() + ",\n");
							}
							stringBuilder.append("]");
							openAlert(stringBuilder.toString());
						} catch (Relayr_Exception e) {
							openAlert("Error: " + e.getMessage());
						}
		
						return null;
					}
				}.execute();
			}


	
# Devices

The following endpoints concern  the _Device_ entity. A device is defined as any external entity capable of producing measurements and sending them to the relayr platform or one which is capable of receiving
information from the relayr platform.

### Register a New Device

Registers a device under a user.

- **Endpoint:** Relayr_SDK.registerDevice(`deviceTitle` (string), `modelID` (string - available under the Device Model entity), `firmwareVersion` (string),`description` (string));
- **Returned value:** A relayr_Device object

**Example**

		public void registerDevice(View v) {
		new AsyncTask<Void, Void, Void>() {
			@Override
			protected Void doInBackground(Void... params) {
				try {
					Relayr_Device device = Relayr_SDK.registerDevice("Test device", "ecf6cf94-cb07-43ac-a85e-dccf26b48c86", "1.0.0", "Device created from test app");
					StringBuilder stringBuilder = new StringBuilder();
					stringBuilder.append("[\n");
					stringBuilder.append(device.toString() + ",\n");
					stringBuilder.append("]");
					openAlert(stringBuilder.toString());
				} catch (Relayr_Exception e) {
					openAlert("Error: " + e.getMessage());
				}
	
				return null;
			}
		}.execute();
	}


### Get A Specific Device

Retrieves a specific device according to the ID provided

- **Endpoint:**  Relayr_SDK.getDeviceById(devId); 
- **Returned value:** A relayr_Device object  

**Example**

		public void listDevice(View v) {
		new AsyncTask<Void, Void, Void>() {
			@Override
			protected Void doInBackground(Void... params) {
				try {
					String devId = "b2c8fdff-736b-416e-ab6d-348ec16a6bd8"
					Relayr_Device device = Relayr_SDK.getDeviceById(devId);
					StringBuilder stringBuilder = new StringBuilder();
					stringBuilder.append("[\n");
					stringBuilder.append(device.toString() + ",\n");
					stringBuilder.append("]");
					openAlert(stringBuilder.toString());
				} catch (Relayr_Exception e) {
					openAlert("Error: " + e.getMessage());
				}

				return null;
			}
		}.execute();
	}

### Update A Device

Updates the device's information. The method receives a collection of attributes to be modified. The attributes available for modifications are: `description`, `title` and `model`, all are of type _string_.

- **Endpoint:**  Relayr_SDK.updateDeviceAttributesById( devId,newAttributes);
- **Returned value:** A relayr_Device object 

**Example**

		
			public void updateDevice(View v) {
			new AsyncTask<Void, Void, Void>() {
			@Override
			protected Void doInBackground(Void... params) {
				try {
				HashMap<String, Object> newAttributes = new HashMap<String, Object>();	newAttributes.put("title", "Temperature of the hall");
					String devId = "b2c8fdff-736b-416e-ab6d-348ec16a6bd8";
					Relayr_Device device = Relayr_SDK.updateDeviceAttributesById( devId,newAttributes);
					StringBuilder stringBuilder = new StringBuilder();
					stringBuilder.append("[\n");
					stringBuilder.append(device.toString() + ",\n");
					stringBuilder.append("]");
					openAlert(stringBuilder.toString());
				} catch (Relayr_Exception e) {					
				openAlert("Error: " + e.getMessage());
				}
				return null;
			}
		}.execute();
		} 
	


### Connect a Device to an App

Connects a device to an app. Once a device is connected, its readings can be retrieved and modified. The method receives the Id of the device as a parameter.

- **Endpoint:** Relayr_SDK.connectDeviceToApp(devId);
- **Returned value:** None

**Example**

		public void connectDeviceToApp(View v) {
			new AsyncTask<Void, Void, Void>() {
				@Override
				protected Void doInBackground(Void... params) {
					try {
						String devId = "b2c8fdff-736b-416e-ab6d-348ec16a6bd8"
						boolean done = Relayr_SDK.connectDeviceToApp(devId);
						if (done) {
							StringBuilder stringBuilder = new StringBuilder();
							stringBuilder.append("[\n");
							stringBuilder.append( "Done\n");
							stringBuilder.append("]");
							openAlert(stringBuilder.toString());
						}
					} catch (Relayr_Exception e) {
						openAlert("Error: " + e.getMessage());
					}
					return null;
				}
			}.execute();
		}

	
### Disconnect a Device from an App
Disconnects a device from an app. Once disconnected, the device's readings will no longer be readable or modifiable. The method receives the Id of the device as a parameter.

- **Endpoint:** Relayr_SDK.disconnectDeviceFromApp(devId); 
- **Returned value:** None

**Example**

			public void disconnectDeviceToApp(View v) {
				new AsyncTask<Void, Void, Void>() {
					@Override
					protected Void doInBackground(Void... params) {
						try {
							String devId =  "b2c8fdff-736b-416e-ab6d-348ec16a6bd8";
							boolean done = Relayr_SDK.disconnectDeviceFromApp(devId);
							if (done) {
								StringBuilder stringBuilder = new StringBuilder();
								stringBuilder.append("[\n");
								stringBuilder.append( "Done\n");
								stringBuilder.append("]");
								openAlert(stringBuilder.toString());
							}
						} catch (Relayr_Exception e) {
							openAlert("Error: " + e.getMessage());
						}
						return null;
					}
				}.execute();
			} 

### List All Devices Connected to an App
Retrieves a list of all devices connected to the app. As a user may disconnect a device, it is advisable to check for the presence of a device before trying to connect to it, at least once per session.

- **Endpoint:**  Relayr_SDK.listClientDevices(); type: JSONArray
- **Returned value:** An Array with all devices connected to an app, matching the specified filter



## Readings

### Subscribe to a specific Device Channel

Subscribes the user to a specific device channel which allows receiving the device's readings. 

- **Endpoint:** webSocket.subscribe(channel, new WebSocketCallback(); 
- **Returned value:** None
 
		 	WebSocket webSocket = new WebSocket(authKey, cipherKey);
		        webSocket.subscribe(channel, new WebSocketCallback() {
		            @Override
		            public void connectCallback(Object message) {
				Log.d(TAG, “CONNECTED: “ + message.toString());
		            }
		
		            @Override
		            public void disconnectCallback(Object message) {
		           Log.d(TAG, “DISCONNECTED: “ + message.toString());
		            }
		
		            @Override
		            public void reconnectCallback(Object message) {
		           Log.d(TAG, “RECONNECTED: “ + message.toString());
		            }
		
		            @Override
		            public void successCallback(Object message) {
		           Log.d(TAG, “SUCCESS: “ + message.toString());
		            }
		
		            @Override
		            public void errorCallback(Throwable error) {
		           Log.e(TAG, “ERROR: “ + error.getMessage());
		            }
		        });


The following endpoints are used for the retrieval and modification of a device's readings.

### Get Device Reading

Retrieves a device's current reading.

- **Endpoint** retrieveDevice(String deviceId); type: JSONObject
- **Returned value** A JSONobject with the current data and and the current values of the device.

**Example** 


### Modify Device Readings 
A device is capable of sending its readings but also of receiving data that would initiate an action, for example flashing a LED. Data is sent as a HashMap, omitted keys are ignored leaving their values unchanged.

- **Endpoint:** modifyDevice(String deviceId, HashMap<String, Object> values); type: boolean
- **Returned value:** TRUE if the action was successfully completed, False otherwise.

**Example**


## Device Configuration

The following three endpoints are used to read, update and reset the configuration of a particular device. 

### Get a Device's Current Configuration

Retrieves a device's current configuration. It will typically include a **frequency** value which defines how often the device transmits its readings and a **Threshold** value the amount of change which would require data transmission. 

- **Endpoint:** retrieveDeviceConfiguration(String deviceId); type:JSONObject
- **Returned value:** A JSONobject with the device's configuration info.

**Example**


### Configure A Device

Changes the existing configuration of a device. The configuration changes are delivered as a HashMap, omitted keys are ignored, leaving their values unchanged.

- **Endpoint:** configureDevice(String deviceId, HashMap<String, Object> configuration); type: boolean
- **Returned value:** TRUE if the action was successfully completed, FALSE otherwise.

**Example**

	
### Reset the Configuration of a Device

Resets the configuration of a device to the default one.

- **Endpoint** deleteDevice(String deviceId); type: boolean
- **Returned value** TRUE if the action was successfully completed, FALSE otherwise.

**Example**

# Device Models

The following endpoints concern the Device Model entity. The device model holds information about the type of readings a device collects and its manufacturer.

### Get Device Models

Returns all Device Models defined on the relayr platform

- **Endpoint:** Relayr_SDK.getAllDeviceModels();
- **Returned value:** An array of relayr_ModelDefinition objects

**Example**

			public void listAllDeviceModels(View v) {
		new AsyncTask<Void, Void, Void>() {
			@Override
			protected Void doInBackground(Void... params) {
				try {
					ArrayList<Relayr_DeviceModelDefinition> models = Relayr_SDK.getAllDeviceModels();
					StringBuilder stringBuilder = new StringBuilder();
					stringBuilder.append("[\n");
					for (Relayr_DeviceModelDefinition model:models) {
						stringBuilder.append(model.toString() + ",\n");
					};
					stringBuilder.append("]");
					openAlert(stringBuilder.toString());
				} catch (Relayr_Exception e) {
					openAlert("Error: " + e.getMessage());
				}
				return null;
			}
		}.execute();
	}


### Get A Single Device Model

Returns a specific Device Model by its ID

- **Endpoint:** Relayr_SDK.getDeviceModelById(DeviceModelId);
- **Returned value:** A relayr_ModelDefinition object

**Example**

			public void getDeviceModel(View v) {
		new AsyncTask<Void, Void, Void>() {
			@Override
			protected Void doInBackground(Void... params) {
				try {
					Relayr_DeviceModelDefinition model = Relayr_SDK.getDeviceModelById("ecf6cf94-cb07-43ac-a85e-dccf26b48c86");
					StringBuilder stringBuilder = new StringBuilder();
					stringBuilder.append("[\n");
					stringBuilder.append(model.toString() + ",\n");
					stringBuilder.append("]");
					openAlert(stringBuilder.toString());
				} catch (Relayr_Exception e) {
					openAlert("Error: " + e.getMessage());
				}

				return null;
			}
		}.execute();
	}
