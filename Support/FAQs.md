
<h1 fullpage>FAQs </h1>

<h2 class="CollapseHeader">Getting Started</h2>
<div class="collapse">
<h3> Ok I got the WunderBar, what do I do now?</h3>

<p>We are happy to hear that you are ready to bring some things to life!

What you will need to do to get started, is first onboard your WunderBar using the <a href="https://developer.relayr.io/dashboard/onboarding"> relayr OnBoarding app </a>, create an app in the <a href="https://developer.relayr.io/dashboard/apps/myApps">App section</a> in order to receive your app key and then follow the <a href="https://developer.relayr.io/documents/Android/Reference">Android SDK</a> or the <a href="https://developer.relayr.io/documents/iOS/Reference">iOS SDK</a> instructions in order to build your first App!</p> 

<h3> Where can I get some information about the relayr architecture? </h3>

<p>We are so glad you asked! check out our <a href="https://developer.relayr.io/documents/Welcome/Platform" target="_blank"> Cloud Platform introduction </a> for an overview of our architecture.</p>

<h3> What is OnBoarding? why do I need it?</h3>

<p>OnBoarding is your way of introducing your WunderBar to the relayr platform. Click <a href="https://developer.relayr.io/documents/Welcome/OnBoarding" target="_blank">here</a> to read more about the process.</p>

<h3> I am a Web developer. Can I still use the relayr platform?</h3>

<p>Of course! Don't worry, we haven't forgotten you. Check out our <a href="https://developer.relayr.io/documents/WEB/WebDevelopers" target="_blank">Web section </a> for more information. Our code samples will help you get started.</p>

<h3> Which service is relayr using for data distribution?</h3>

<p>We are using PubNub as our data distribution service. You can learn more about our usage of PubNub <a href="https://developer.relayr.io/documents/PubNub/Reference" target="_blank">here</a>.</p>
</div>

<h2 class="CollapseHeader"> OnBoarding</h2>

<div class="collapse">

<h3> I'm trying to onBoard my WunderBar but no WunderBar is found. What could be the cause?</h3>

<p>The most common cause for this is Bluetooth being turned off. We use Bluetooth LE in the OnBoarding process. Make sure it is enabled on your phone, before attempting to onboard. 
</p>

<h3> I would like to Onboard my WunderBar - Should I break the sensor modules off the main board?</h3>

<p>As a rule of thumb we recommend <strong>not to</strong> break the sensor modules off the main circuit until specifically indicated to do so in the steps of the OnBoarding app. Just sit back and let the app tell you exactly what to do</p>

<h3> Can I OnBoard multiple WunderBars at the same time?</h3>

<p>Unfortunately at this time, onboarding multiple WunderBars <strong>at the same time</strong> is not an available option. On the <em>Android</em> app you have the option to onboard multiple WunderBars, just not simultaneously. On the <em>iOS</em> app, this feature, is not yet available. It will be implemented in the next release of the app.</p>

<h3> I am using the <em>Android</em> relayr OnBoarding app and I get a web error after log in - is this normal?</h3>

<p>Well... every app has its tiny bugs. This is unfortunately one of them. The web error message will show up for less than a second. It does not affect the onboarding process. Already fixed in the next version of the app.</p>

<h3> I am using the <em>Android</em> relayr OnBoarding app but my sensors are not detected</h3>

<p>This could happen due to Bluetooth Low Energy which we utilize in order to connect to the sensor modules during the onboarding process. To resolve this issue you may need to disconnect your WunderBar from its power supply, connect it again and start the onboarding process again by clicking the onboarding button as indicated by the app.</p>

<h3> Can I have multiple WunderBars onboarded and view their readings?</h3>

<p>The answer to this question depends on the specific OnBoarding app you are using.</p> 

<p>On the <strong>Android</strong> app this feature has been implemented and you are able to toggle between your onboarded WunderBars. </p>

<p>On the <strong>iOS</strong> app, since multiple OnBoarding is currently not available, you will only be able to view one WunderBar.</p>

<h3> I am using the <em>iOS</em> relayr OnBoarding app but my sensors are not all detected</h3>

<p>This is also a result of the limitations of Bluetooth LE. Try logging out of the application, and then log back in again. If this does not resolve the issue, disconnect the WunderBar from the power supply and then plug it back in and start the OnBoarding process again.</p>

<h3> I am using the <em>iOS</em> relayr OnBoarding app. I have successfully onboarded my WunderBar but I still can't see data on all channels</h3>

<p>This is a known issue which has already been fixed in the next version of the app. To resolve this issue, remove the app completely from the background tray. Keep the WunderBar plugged and restart the application.</p>

<h3> The onboarding app cannot locate one of the sensors. What should I do?</h3>

<p>If The onboarding app fails to locate one of the sensors it will asks you if the LED on the sensor is blinking. In this case, press the button on the relevant sensor (taking it out of onboarding mode) then press it a second time (putting it back in onboarding mode) and hit retry on the app.</p>	

<h3> I seem to have lost my WiFi connection after trying to onBoard. What could be the cause?</h3>

<p>When using the the Android OnBoarding App, a temporary local network is initiated by the Master Module, to facilitate the OnBoarding process. If the OnBoarding app is stopped (killed) mid process, without having successfully onboarded the WunderBar, your phone may stay connected to this local network. To resolve this, make sure to reconnect to your normal WiFi network. </p>
</div>

<h2 class="CollapseHeader">Hardware and Firmware</h2>
<div class="collapse">
<h3> Those blinking LEDs, what do they mean?</h3>

<p>We realize that the on and off blinking and flashing statuses can be quite confusing, so here is a little guide to help you distinguish one blink from the other.</p>

<h4>Master Module LEDs</h4>
<p>The Master Module includes 3 LEDs:</p>

<ol>
<li><p><strong>The WiFi LED</strong>: Located at the edge of the board, between the USB connector and the Inductor marked with a <strong>K</strong>:
   When it is ON, the Master Module is NOT connected to WiFi. When this LED is off it is an indication that the Master Module has successfully connected to the WIFi network.</p></li>

<li><p><strong>The Battery charger LED</strong>: located between the reset button and battery IC. It indicates the state of	the battery charging circuit. This LED will blink after 4 hours of use, if no battery is connected. When the battery is being charged this LED will stay on.</p></li>

<li><p><strong>The Master Module Mode Indicator</strong>: Located next to the Kinetis chip.</p>
 
	<ul>
	<li><p><strong>Blinking once per second</strong>: During Onboarding Mode indicating that the WunderBar is waiting to be onboarded.</p></li>
   
	<li><p><strong>Constantly ON</strong>: Indicating that the Master Module is in Device Firmware Update (DFU) Bootloader Mode and that it is waiting for new firmware. This mode can be initiated with a long press on the onboarding button.</p></li>

   	<li><p><strong>OFF</strong>: The common state of this LED. When the Master Module is connected to the platform (broker), this LED will stay off most of the time, and will only shortly blink every time a sensor publishes data.</p></li></ul></li>
</ol>


<h4>Sensor Module LED</h4>

<p>All sensor modules have one led each:</p>

<ul>
<li><p><strong>LED Blinking once a second</strong>: During Onboarding Mode. The module is advertising the Onboarding service, to be written to by the Onboarding App.</p></li>

<li><p><strong>LED ON</strong>: Indicating that the sensor is in Over The Air- Device Firmware Update (OTA-DFU) mode. The DeviceFirmwareUpdate application is running, waiting for the Android/iOS app to send new firwmare.</p></li>

<li><p><strong>LED OFF</strong>: Normal operation.</p></li>
 
<li><p><strong>Short Blink</strong>: The sensor module will blink once when to indicate that it is connected to the App or the Master Module.</p></li>
</ul>

<h3> How can I upgrade the Firmware on my Sensor Modules?</h3>

<p>To upgrade your Sensor Modules' Firmware you can use the <a href="https://play.google.com/store/apps/details?id=no.nordicsemi.android.mcp&hl=en" target="_blank"> nrf Master Control Panel</a>. We are planning on integrating the Firmware upgrade into the OnBoarding app in a future release.</p>

<h3> The Master Module comes with a battery - Does it have to be connected to a power supply at all times?</h3>

<p>We've discovered that due to a bug in the firmware, the battery charger of the Master Module, which also serves as a voltage regulator, enters an erratic state every ~4 hours, when not connected to a power supply - which eventually results in it disconnecting the WunderBar and breaking off any data transmission. This is why we are currently shipping the Master Modules with a battery, which will prevent it from getting to this state.</p>

<h3> Can I send audio to the relayr cloud, using the noise sensor?</h3>

<p>Our noise sensor is unable to send audio to the cloud as it is <em>not</em> a microphone. The output of this sensor is the relative level of noise at the location measured </p>

</div>

 