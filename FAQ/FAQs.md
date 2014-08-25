# FAQs 

## Getting Started

### Ok I got the WunderBar, what do I do now?

We are happy to hear that you are ready to bring some things to life!

What you will need to do to get started, is first onboard your WunderBar using the <a href="https://developer.relayr.io/dashboard/onboarding"> relayr OnBoarding app </a>, create an app in the <a href="https://developer.relayr.io/dashboard/apps/myApps">App section</a> in order to receive your app key and then follow the <a href="https://developer.relayr.io/documents/Android/Reference">Android SDK</a> or the <a href="https://developer.relayr.io/documents/iOS/Reference">iOS SDK</a> instructions in order to build your first App! 

### Where can I get some information about the relayr architecture?

We are so glad you asked! check out our <a href="https://developer.relayr.io/documents/Welcome/Platform" target="_blank"> Cloud Platform introduction </a> for an overview of our architecture.

### What is OnBoarding? why do I need it?

OnBoarding is your way of introducing your WunderBar to the relayr platform. Click <a href="https://developer.relayr.io/documents/Welcome/OnBoarding" target="_blank">here</a> to read more about the process.

### I am a Web developer. Can I still use the relayr platform?

Of course! Don't worry, we haven't forgotten you. Check out our <a href="https://developer.relayr.io/documents/WEB/WebDevelopers" target="_blank">Web section </a> for more information. Our code samples will help you get started.

### Which service is relayr using for data distribution?

We are using PubNub as our data distribution service. You can learn more about our usage of PubNub <a href="https://developer.relayr.io/documents/PubNub/Reference" target="_blank">here</a>.

## OnBoarding

### I'm trying to onBoard my WunderBar but no WunderBar is found. What could be the cause?

The most common cause for this is Bluetooth being turned off. We use Bluetooth LE in the OnBoarding process. Make sure it is enabled on your phone, before attempting to onboard. 

### I would like to Onboard my WunderBar - Should I break the sensor modules off the main board?

As a rule of thumb we recommend **not to** break the sensor modules off the main circuit until specifically indicated to do so in the steps of the OnBoarding app. Just sit back and let the app tell you exactly what to do

### Can I OnBoard multiple WunderBars at the same time?

Unfortunately at this time, onboarding multiple WunderBars **at the same time** is not an available option. On the *Android* app you have the option to onboard multiple WunderBars, just not simultaneously. On the *iOS* app, this feature, is not yet available. It will be implemented in the next release of the app.

### I am using the *Android* relayr OnBoarding app and I get a web error after log in - is this normal?

Well... every app has its tiny bugs. This is unfortunately one of them. The web error message will show up for less than a second. It does not affect the onboarding process. Already fixed in the next version of the app.

### I am using the *Android* relayr OnBoarding app but my sensors are not detected

This could happen due to Bluetooth Low Energy which we utilize in order to connect to the sensor modules during the onboarding process. To resolve this issue you may need to disconnect your WunderBar from its power supply, connect it again and start the onboarding process again by clicking the onboarding button as indicated by the app.

### Can I have multiple WunderBars onboarded and view their readings?

The answer to this question depends on the specific OnBoarding app you are using. 

On the **Android** app this feature has been implemented and you are able to toggle between your onboarded WunderBars. 

On the **iOS** app, since multiple OnBoarding is currently not available, you will only be able to view one WunderBar.

### I am using the *iOS* relayr OnBoarding app but my sensors are not all detected

This is also a result of the limitations of Bluetooth LE. Try logging out of the application, and then log back in again. If this does not resolve the issue, disconnect the WunderBar from the power supply and then plug it back in and start the OnBoarding process again.

### I am using the *iOS* relayr OnBoarding app. I have successfully onboarded my WunderBar but I still can't see data on all channels

This is a known issue which has already been fixed in the next version of the app. To resolve this issue, remove the app completely from the background tray. Keep the WunderBar plugged and restart the application.

### The onboarding app cannot locate one of the sensors. What should I do?

If The onboarding app fails to locate one of the sensors it will asks you if the LED on the sensor is blinking. In this case, press the button on the relevant sensor (taking it out of onboarding mode) then press it a second time (putting it back in onboarding mode) and hit retry on the app.	

### I seem to have lost my WiFi connection after trying to onBoard. What could be the cause?

When using the the Android OnBoarding App, a temporary local network is initiated by the Master Module, to facilitate the OnBoarding process. If the OnBoarding app is stopped (killed) mid process, without having successfully onboarded the WunderBar, your phone may stay connected to this local network. To resolve this, make sure to reconnect to your normal WiFi network. 


## Hardware and Firmware

### Those blinking LEDs, what do they mean?

We realize that the on and off blinking and flashing statuses can be quite confusing, so here is a little guide to help you distinguish one blink from the other.

**Master Module LEDs**
The Master Module includes 3 LEDs:

1. **The WiFi LED**: Located at the edge of the board, between the USB connector and the Inductor marked with a **K**:
   When it is ON, the Master Module is NOT connected to WiFi. When this LED is off it is an indication that the Master Module has successfully connected to the WIFi network.

2. **The Battery charger LED**: located between the reset button and battery IC. It indicates the state of	the battery charging circuit. This LED will blink after 4 hours of use, if no battery is connected. When the battery is being charged this LED will stay on.

3. **The Master Module Mode Indicator**: Located next to the Kinetis chip.
 
	**Blinking once per second**: During Onboarding Mode indicating that the WunderBar is waiting to be onboarded.
   
	**Constantly ON**: Indicating that the Master Module is in Device Firmware Update (DFU) Bootloader Mode and that it is waiting for new firmware. This mode can be initiated with a long press on the onboarding button.

   	**OFF**: The common state of this LED. When the Master Module is connected to the platform (broker), this LED will stay off most of the time, and will only shortly blink every time a sensor publishes data.


**Sensor Module LED**

All sensor modules have one led each:

**LED Blinking once a second**: During Onboarding Mode. The module is advertising the Onboarding service, to be written to by the Onboarding App.

**LED ON**: Indicating that the sensor is in Over The Air- Device Firmware Update (OTA-DFU) mode. The DeviceFirmwareUpdate application is running, waiting for the Android/iOS app to send new firwmare.

**LED OFF**: Normal operation.
 
**Short Blink**: The sensor module will blink once when to indicate that it is connected to the App or the Master Module.


### How can I upgrade the Firmware on my Sensor Modules?

To upgrade your Sensor Modules' Firmware you can use the <a href="https://play.google.com/store/apps/details?id=no.nordicsemi.android.mcp&hl=en" target="_blank"> nrf Master Control Panel</a>. We are planning on integrating the Firmware upgrade into the OnBoarding app in a future release.

### The Master Module comes with a battery - Does it have to be connected to a power supply at all times?

We've discovered that due to a bug in the firmware, the battery charger of the Master Module, which also serves as a voltage regulator, enters an erratic state every ~4 hours, when not connected to a power supply - which eventually results in it disconnecting the WunderBar and breaking off any data transmission. This is why we are currently shipping the Master Modules with a battery, which will prevent it from getting to this state.


 