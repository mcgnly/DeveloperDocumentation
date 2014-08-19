# Device Endpoints

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

