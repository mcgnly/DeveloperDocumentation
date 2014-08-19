## Device Model Endpoints

The following endpoints concern the *Device Model* entity. The device model holds information about the type of readings a device collects and its manufacturer.

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
