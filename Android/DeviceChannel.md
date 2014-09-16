# Subscribing to a Device Channel - Example

The following code sequence is an example of how the Android SDK could be used to select a device from a list of onboarded devices and to subscribe to the specific device channel. 

Subscribing to a device channel means completing the "handshake" between the App and the relayr platform and receiving all necessary credentials in order to start receiving data from a specific device.

In the example below we've used the Humidity and Temperature sensor.
We've divided the complete code into two snippets as to make it easier to explain which endpoint is being called for at every stage.

## Stage 1 - Selecting a specific user device out of a list of existing devices

This stage includes a number of calls which enable you to first query for a user, then obtain all the transmitters listed under the user. We've chosen to select the first transmitter we find and continue with it- just for the sake of the example. 

What we do next is check whether a Temperature sensor is among the sensors related to the transmitter and select it. 

The last call in the example is the `subscribeForTemperatureUpdates(device)` - which is detailed in the second code snippet.  	

In this stage the main class which is being called for is the **RelayrApi** class which  incorporates a wrapped version of all <a href="https://developer.relayr.io/dashboard/api"> relayr API calls </a>. This class cannot be accessed directly. It is always called for by the main **RelayrSdk** class, as seen below: 

	mTemperatureDeviceSubscription = RelayrSdk.getRelayrApi()
            .getUserInfo()
            .flatMap(new Func1<User, Observable<List<Transmitter>>>() {
                @Override
                public Observable<List<Transmitter>> call(User user) {
                    return RelayrSdk.getRelayrApi().getTransmitters(user.id);
                }
            })
            .flatMap(new Func1<List<Transmitter>, Observable<List<TransmitterDevice>>>() {
                @Override
                public Observable<List<TransmitterDevice>> call(List<Transmitter> transmitters) {
                    // This is a naive implementation. Users may own multiple WunderBars or different
                    // kinds of transmitters.
                    if (transmitters.isEmpty())
                        return Observable.from(new ArrayList<List<TransmitterDevice>>());
                    return RelayrSdk.getRelayrApi().getTransmitterDevices(transmitters.get(0).id);
                }
            })
            .filter(new Func1<List<TransmitterDevice>, Boolean>() {
                @Override
                public Boolean call(List<TransmitterDevice> devices) {
                    // Check whether there is a thermometer among the devices listed under the transmitter.
                    for (TransmitterDevice device : devices) {
                        if (device.model.equals(DeviceModel.TEMPERATURE_HUMIDITY.getId())) {
                            return true;
                        }
                    }
                    return false;
                }
            })
            .flatMap(new Func1<List<TransmitterDevice>, Observable<TransmitterDevice>>() {
                @Override
                public Observable<TransmitterDevice> call(List<TransmitterDevice> devices) {
                    for (TransmitterDevice device : devices) {
                        if (device.model.equals(DeviceModel.TEMPERATURE_HUMIDITY.getId())) {
                            return Observable.just(device);
                        }
                    }
                    return null; 
                }
            })
            .subscribeOn(Schedulers.io())
            .observeOn(AndroidSchedulers.mainThread())
            .subscribe(new Subscriber<TransmitterDevice>() {
                @Override
                public void onCompleted() {

                }

                @Override
                public void onError(Throwable e) {
                    Toast.makeText(ThermometerDemoActivity.this, R.string.something_went_wrong,
                            Toast.LENGTH_SHORT).show();
                }

                @Override
                public void onNext(TransmitterDevice device) {
                    subscribeForTemperatureUpdates(device);
                }
            });

## Stage 2 - Subscribing to a device channel

This stage is a specification of the `subscribeForTemperatureUpdates(device)` method which enables an app to subscribe to a device channel.

The main class called for in this stage is the **WebSocket** class which handles all calls related to device channel subscription and obtaining readings. Much like the RelayrApi class, the WebSocket class is also reachable only via the main RelayrSdk class:

	private void subscribeForTemperatureUpdates(TransmitterDevice device) {
        mWebSocketSubscription = RelayrSdk.getWebSocketClient()
                .subscribe(device, new Subscriber<Object>() {

                    @Override
                    public void onCompleted() {

                    }

                    @Override
                    public void onError(Throwable e) {
                        Toast.makeText(ThermometerDemoActivity.this, R.string.something_went_wrong,
                                Toast.LENGTH_SHORT).show();
                    }

                    @Override
                    public void onNext(Object o) {
                        Reading reading = new Gson().fromJson(o.toString(), Reading.class);
                        mTemperatureValueTextView.setText(reading.temp + "˚C");
                    }
                });
    }

