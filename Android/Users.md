# User Endpoints

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

