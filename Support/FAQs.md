

<h1 fullpage>FAQs </h1>

<h2 class="collapseHeader">Getting Started</h2>
<div class="collapse">
<h3> Ok I got the WunderBar, what do I do now?</h3>

<p>We are happy to hear that you are ready to bring some things to life!

What you will need to do to get started, is first onboard your WunderBar using the <a href="https://developer.relayr.io/dashboard/onboarding"> relayr OnBoarding app </a>, create an app in the <a href="https://developer.relayr.io/dashboard/apps/myApps">App section</a> in order to receive your app key and then follow the <a href="https://developer.relayr.io/documents/Android/Reference">Android SDK</a> or the <a href="https://developer.relayr.io/documents/Apple/Introduction">iOS&OSX SDK</a> instructions in order to build your first App!</p>

<h3> Where can I get my App Id? </h3> 

<p>Once you've created an app on the <a href="https://developer.relayr.io/dashboard/apps/myApps">Apps section</a>, you can always return to the Apps section and see the credentials of your app.</p>

<h3> How do I generate an OAuth token for my App? </h3>

<p>To generate a token for your app you can simply follow the instructions in our <a href="https://developer.relayr.io/documents/WebDev/OAuthToken">Generate OAuth Token </a>page.</p>

<h3> Where can I get my User Id? </h3>

<p> To get your user Id simply log in to the dashboard, hover over your user name in the header menu > Select Account > Settings. You will find your user ID there.</p>


<h3> Where can I get some information about the relayr architecture? </h3>

<p>We are so glad you asked! check out our <a href="https://developer.relayr.io/documents/Welcome/Platform" target="_blank"> Cloud Platform introduction </a> for an overview of our architecture.</p>

<h3> What is OnBoarding? why do I need it?</h3>

<p>OnBoarding is your way of introducing your WunderBar to the relayr platform. Click <a href="https://developer.relayr.io/documents/Welcome/OnBoarding" target="_blank">here</a> to read more about the process.</p>

<h3> Where can I get my Device Id? </h3>
<p>Once you've onboarded your WunderBar, to get your Devices Ids, go to the Devices section and click on the small 'settings' icon of any of your sensors to get its Id</p>

</div>

<h2 class="collapseHeader"> The OnBoarding Process</h2>

<div class="collapse">

<h2 class="collapseHeader"> General</h2>

<div class="collapse">

<h3> Can I onboard a WunderBar using a <em>WPA2-Enterprise</em> Network?</h3>

<p>The short answer is no. The Master Module can only connect to a <strong>WPA2-Personal</strong> network.</p>

<h3> I'm trying to onboard my WunderBar and I experience difficulties - what should I check?</h3>

<p>
Before initiating the OnBoarding process have a look at our <a href="https://www.youtube.com/watch?v=SDG7qpYgoZc"><strong>OnBoarding Do's and Dont's video</strong></a>  for valuable information which could save you a lot of frustration and ensure that your onboarding is successful.</p>

<iframe width="560" height="315" src="//www.youtube.com/embed/SDG7qpYgoZc" frameborder="0" allowfullscreen></iframe>

<p>
OnBoarding could be severely impacted by the environment in which you choose to onboard, more precisely by the state of the WiFi network you choose to use when onboarding.<br/>
<ul>

<li>Optimal conditions for onboarding are a <em><strong>stable WiFi network and one which is not utilized by a large number of users</strong></em> - Trying to onboard in a lecture room with 100 people all using the WiFi network is probably not your best choice for securing a successful onboarding process.</li>
<li>The second factor critical for a successful onboarding is <em><strong>maintaining the the board unbroken</strong></em>. We recommend not to break any of the sensors off the board prior to successfully onboarding it. </li>
<li>A Third aspect which should be examined prior to onboarding is the <em><strong>WiFi password</strong></em> of the Network used during the Onboarding process. The password should not exceed 18 characters and all characters should be <strong>A-Z, a-z and 0-9</strong> characters.</li>
<li>Another WiFi related issue which should be verified is that <strong><em>the WiFi network used is a 2.4 GHz one</em></strong>.</li>
<li>When the above conditions are met the things that you should make sure is that the <em><strong>Master Module and all other sensors are in OnBoarding mode</strong></em> and that the Master Module is connected to the WiFi network. Please note that in case it is not connected, the WiFi LED (Located at the edge of the board, between the USB connector and the Inductor marked with a K) will be on.</li>

</ul>    
</p>

<h3>I have a Fritzbox, my WiFi network is a 2.4 Ghz one- but I still cannot onboard - What could be the issue?</h3>

<p>If you've verified that your network settings meet all the above requirements for onboarding, you may be experiencing a problem similar to what is described in <a href="https://forum.relayr.io/t/wunderbar-wont-connect-to-wifi/471/14">this forum post</a>. </p>

<h3> How can I check whether my network is a 2.4 GHz one?</h3>

<p>You could check this with a single command run from your PC's command-line interface.
<br />
For <strong>Windows</strong> use: <code>netsh wlan show all</code><br/>
For <strong>Mac OS</strong> use: <code>/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport -I</code>
<br />
The output of the command includes a <strong>channel</strong> parameter. If the channel is below 15 (i.e. 1-14), the network the computer is connected to is a 2.4 GHz one.  
</p>

<h3> I've accidentally broken the board- can I still onboard?</h3>
<p> We recommend not to break the board prior to onboarding, however, we have been successful at onboarding abroken board as well. Please follow the instructions below in order to onboard your broken board:

<ol>
<li>Insert the batteries to all sensors (all but the Grove module, which does not have such a slot. I'm afraid you will be able to onboard all but the Grove module at this stage precisely for this reason)</li>

<li>Connect the Master Module to a power supply via the USB cable <em>and</em> to the battery shipped with the board. It is vital that both are connected.</li>

<li>Place the sensors as close to the Master Module as possible and in very close proximity to your phone and commence the OnBoarding process - by starting up the OnBoarding app and following the instructions in it.</li>

</ol>

<h3> I would like to Onboard my WunderBar - Should I break the sensor modules off the main board?</h3>

<p>As a rule of thumb we recommend <strong>not to</strong> break the sensor modules off the main circuit until specifically indicated to do so in the steps of the OnBoarding app. Just sit back and let the app tell you exactly what to do</p>

<h3> Can I OnBoard multiple WunderBars at the same time?</h3>

<p>Unfortunately at this time, onboarding multiple WunderBars <strong>at the same time</strong> is not an available option. On the <em>Android</em> app you have the option to onboard multiple WunderBars, just not simultaneously. On the <em>iOS</em> app, this feature, is not yet available. It will be implemented in the next release of the app.</p>

<h3> Can I have multiple WunderBars onboarded and view their readings?</h3>

<p>The answer to this question depends on the specific OnBoarding app you are using.</p> 

<p>On the <strong>Android</strong> app this feature has been implemented and you are able to toggle between your onboarded WunderBars. </p>

<p>On the <strong>iOS</strong> app, since multiple OnBoarding is currently not available, you will only be able to view one WunderBar.</p>

<h3>Are there any debugging tools which I could use to see why the OnBoarding process is unsuccessful?</h3>

<p>At the moment, we unfortunately do not provide any debugging tools for any of the applications. We are still at the early stages of development and these will be integrated in the upcoming versions of the app. We apologize in advanced for this limitation.</p>

</div>



<h2 class="collapseHeader"> Android</h2>

<div class="collapse">

<h3>I have Bluetooth enabled but the onboarding app keeps  telling me it is off - Why?</h3>
<p>This is most probably due to the version of Android you have on your phone. Please note that <strong><em>onboarding the WunderBar is only available on phones running Android 4.3 and up</em></strong>. In case you are using a phone with an earlier version you could still see the data from the sensors, however, the actual onboarding would need to be done on a phone with the above version. </p>

<h3> I am using the <em>Android</em> relayr OnBoarding app and I get a web error after log in - is this normal?</h3>

<p>Well... every app has its tiny bugs. This is unfortunately one of them. The web error message will show up for less than a second. It does not affect the onboarding process. Already fixed in the next version of the app.</p>

<h3> I am using the <em>Android</em> relayr OnBoarding app but my sensors are not detected</h3>

<p>This could happen due to Bluetooth Low Energy which we utilize in order to connect to the sensor modules during the onboarding process. To resolve this issue you may need to disconnect your WunderBar from its power supply, connect it again and start the onboarding process again by clicking the onboarding button as indicated by the app.</p>

<h3> The onboarding app cannot locate one of the sensors. What should I do?</h3>
<p>If The onboarding app fails to locate one of the sensors it will asks you if the LED on the sensor is blinking. In this case, press the button on the relevant sensor (taking it out of onboarding mode) then press it a second time (putting it back in onboarding mode) and hit retry on the app.</p>

<h3> I seem to have lost my WiFi connection after trying to onBoard. What could be the cause?</h3>

<p>When using the the Android OnBoarding App, a temporary local network is initiated by the Master Module, to facilitate the OnBoarding process. If the OnBoarding app is stopped (killed) mid process, without having successfully onboarded the WunderBar, your phone may stay connected to this local network. To resolve this, make sure to reconnect to your normal WiFi network. </p>

<h3>
I am using a Samsung S-series device and I cannot complete the OnBoarding process. What could be the cause?</h3>

<p>We have identified a number of BLE related issues which are experienced on devices manufactured by Samsung, which prevent the Onboarding process from completing successfully. These issues have been fixed in version 1.0.22 of the Android Manage App. If you have a Samsung phone and you are experiencing such issues, please make sure to install the latest app version, currently available on the <a href="https://play.google.com/store/apps/details?id=io.relayr.wunderbar">Play store</a> </p>
</div>

<h2 class="collapseHeader"> Device Specific Issues</h2>

<div class="collapse">

<h3>
I am using the Samsung Galaxy S3 and I cannot complete the OnBoarding process. What could be the cause?</h3>

<p>You may have a specific setting on - which interferes with Onboarding. Click <a href="https://developer.relayr.io/documents/Support/GalaxyS3">here </a> for a troubleshooting guide.</p>

<h3>I am using a Nexus 7, I am able to onboard the Master Module but my Sensor Modules cannot be configured</h3>

<p>We have identified an issue which is related to BLE on the Nexus 7. What we recommend in order to secure a successful onboarding process, is to first reboot the device, turn Bluetooth on and make sure devices are being detected and only then attempt to onboard.</p>

</div>

<h2 class="collapseHeader">iOS</h2>

<div class="collapse">
<h3>What is the earliest iOS version I could use the OnBoarding app on?</h3>

<p>The earliest version you could use the iOS OnBoarding app is version 8. Please note: The <strong>Bluetooth version on the device must be 4.0</strong> (Bluetooth Smart). Onboarding is not possible with an earlier Bluetooth version.</strong></p>

<h3> I am using the iOS Onboarding App. Where can I see my sensor data? </h3>

<p>When using the iOS App you are able to see your sensor data either on the <a href="https://developer.relayr.io/dashboard/devices" >Developer Dashboard</a> or by downloading our <a href="https://itunes.apple.com/de/app/tell-me-when/id947289745?l=en&mt=8">TellMeWhen</a> application, which allows you to view your sensor data and get notifications about it based on rules you set. Currently the iOS app is not as feature complete as our Android app, which is why, at the moment viewing data in the app is not available. We are of course working on improving the iOS app so that it has the same capabilities as that of the Android app. </p>

<h3> How do I use the iOS Onboarding App</h3>

<p>Since our Beta currently only includes very limited functionality, please see <a href="https://forum.relayr.io/t/problem-onboarding-ios/176/2?u=dana_relayr">this forum post</a> for instructions how to use the app in order to onboard.</p>

</div></div>

<h2 class="collapseHeader"> Development Tools </h2>
<div class="collapse">

<h3>Where can I get the Android SDK from?</h3>

<p> Please visit our <a href="https://developer.relayr.io/documents/Android/Reference"> Android Section</a> to get started with Android Development </p>

<h3>Where can I get the iOS/OSX SDK from?</h3>

<p> Please visit our <a href="https://developer.relayr.io/documents/Apple/Reference">iOS/OSX Section</a> to get started with iOS/OSX Development </p>

<h3> I am a Web developer. Can I still use the relayr platform?</h3>

<p>Of course! Don't worry, we haven't forgotten you. Check out our <a href="https://developer.relayr.io/documents/WebDev/WebDevelopers" target="_blank">Web section </a> for more information. Our code samples will help you get started.</p>

<h3>Do you have anything for Python developers?</h3>

<p>We sure do! Check out our shiny <a href="https://developer.relayr.io/documents/Python/Introduction">Python Section</a></p>

<h3>Do you have anything for us C# developers?</h3>

<p>We do! Check out our brand new <a href="https://developer.relayr.io/documents/CSharp/Reference">C# section</a></p>

<h3>Do you have anything for <em>Ruby</em> Developers?</h3>
<p> We have not released any official Ruby library, but luckily we have a great community of developers who support us and one of them has created a Gist for Ruby <a href="https://gist.github.com/jShaf/4905ef199885afb691d5">available here</a>. </p>

<h3>I would like to use Eclipse in order to create my Android project, is that possible? </h3>

<p>Officially, the only supported IDE for Android development is Android Studio. However, you can use the hint on <a href="https://forum.relayr.io/t/automatic-sdk-inclusion-fails/423/3">this forum post</a> in order to use Eclipse as well</p>


<h3>I am using the iOS SDK and I need to log in through relayr every time I compile - Is there a way to log in programmatically </h3>

<p>You may want to look at the <a href="https://forum.relayr.io/t/auto-login-programmatically/259">this Forum post</a> to get a bit more insight about this issue</p>

</div>

<h2 class="collapseHeader"> Sensor Data </h2>
<div class="collapse">

<h3>I have successfully onboarded my WunderBar sensors and was able to receive data. All of a sudden, I stopped receiving data from some of them. What could be the cause?</h3>

<p>We are aware of a phenomenon whereby one or some of the sensors, after having been successfully onboarded and having successfully published data, randomly stop publishing data. We are monitoring our servers in order to pinpoint the root cause of this behavior.
</p><p>
If you encounter this phenomenon, please supply us with the IDs of the sensors which have stopped publishing as well as the exact time this behavior was encountered. You can do so by posting to <a href="https://forum.relayr.io/t/sensors-randomly-stop-publishing-data/254">this Forum Topic</a></p>

<h3> In which units is Noise Level measured? </h3>
<p>
Our sensor essentially provides a reading representing the change in noise level over a period of time. The number is a representation of a level rather than a measurement of noise. The output number will increase as the noise level increases and vice versa. In order to provide a reading of the level of noise in decibels a much more elaborate set of tools is required.    
</p>
<h3> Why does the percentage go up when I get close to the proximity sensor? </h3>
<p>The proximity measurement is translated to a percentage in the OnBoarding apps and the Developer Dashboard. The closer an object is to the sensor the higher the percentage is.
</p>
<p>For additional reading about the WunhderBar sensors and a review of the manner in which data is collected please see our <a href="https://developer.relayr.io/documents/Welcome/Sensors">Making Sense of Sensors page</a></p>

<h3> Is it possible to receive sensor values directly from the sensor without using the Master Module?</h3>
<p>Yes! that is possible. Simply set the relevant sensor to BLE mode via the OnBoarding app and start receiving data directly to your phone over BLE.
</p>

<h3> I have successfully onboarded my WunderBar using my work network and was receiving data from it, I came home, connected it and nothing works - why? </h3>
<p>In case you are using one network to onboard and then try and connect the WunderBar to a new network you would need to initiate the "Change WiFi" process on the Onboarding/Manager app prior to being able to see data. The Master module remains "connected" to the old network until the network settings have been changed. Once you initiate the process, follow the instructions on the app, which will guide in connecting to the new network.
</p>

</div>

<h2 class="collapseHeader"> BLE Direct Connection </h2>
<div class="collapse">

<h3> What is BLE Direct Connection?</h3>
<p>BLE direct connection enables you to connect to each of the WunderBar sensor modules over BLE and receive data from them without the mediation of the relayr cloud platform.</p>


<h3> Is BLE Direction Connection available on both Android and iOS apps?</h3>
<p>At the moment the option to connect to the sensor module directly is only available from the <strong>Android Manager</strong> app.</p>

<h3>How do I use BLE Direct Connection?</h3>
<p>In order to use this option, place the relevant sensor in onboarding mode by shortly pressing its button, then from your Android Manager app, swipe to the right and turn on BLE Direct Connection, once the connection is established, you should be able to see the data on the main sensor screen.</p>


<h3>Are there any known issues connecting to the sensor modules over BLE?</h3>
<p>As this connection method is relatively new, we are still at the stages of perfecting it. The main issue which may arise while attempting to connect to the sensor via BLE, is in case the first attempt is unsuccessful, the connection may never be established and fail with a repeating request to place the sensor module in onboarding mode. In this case, we recommend to kill the app completely and disconnect the sensor from it's power supply. Once reconnected, the access via BLE should be successful.  
</p>





</div>

<h2 class="collapseHeader">Hardware and Firmware</h2>
<div class="collapse">
<h3> Those blinking LEDs, what do they mean?</h3>

<p>We realize that the on and off blinking and flashing statuses can be quite confusing, so here is a little guide to help you distinguish one blink from the other.</p>

<h4>Master Module LEDs</h4>
<p>The Master Module includes 3 LEDs:</p>
<img src="assets/Master3.png" width=700px>
<ol>
<li><p><strong>The WiFi LED</strong>: Located at the edge of the board, between the USB connector and the Inductor marked with a <strong>K</strong>:
   When it is ON, the Master Module is NOT connected to WiFi. When this LED is off it is an indication that the Master Module has successfully connected to the WIFi network.</p></li>

<li><p><strong>The Battery charger LED</strong>: located between the reset button and battery IC. It indicates the state of	the battery charging circuit. This LED will blink after 4 hours of use, if no battery is connected. When the battery is being charged this LED will stay on.</p></li>

<li><p><strong>The Master Module Mode Indicator</strong>: Located next to the <a href="https://developer.relayr.io/documents/Welcome/MM"> Kinetis </a> chip.</p>
 
	<ul>
	<li><p><strong>Blinking once per second</strong>: During Onboarding Mode indicating that the WunderBar is waiting to be onboarded.</p></li>
   
	<li><p><strong>Constantly ON</strong>: Indicating that the Master Module is in Device Firmware Update (DFU) Bootloader Mode and that it is waiting for new firmware. This mode can be initiated with a long press on the onboarding button.<br/>
	<strong>In case the Master Module detects a problem connecting to the relayr Cloud</strong> platform, this LED will also be constantly on.</p></li>

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

<h3> What do we mean when we say Frequency?</h3>

<p>One of the fields available under the Device Settings is Frequency. We'd like to take a moment and explain what we mean by it. The Frequency field in this case refers to how often values are being sent from the WunderBar sensor to the Master Module and then to the relayr cloud platform. Hence, we could say that in this sense frequency is the sampling rate or sampling interval and not the rate at which the sensor publishes values. </p>

<h3> Can I flash the Firmware on my Sensor Modules and install something else?</h3>

<p>You are able to flash your Sensors' firmware. Please have a look at <a href="https://developer.relayr.io/documents/HowTos/Flashing">instructions how to perform that</a>. You could install the iBeacon firmware on the sensor modules by following the <a href="https://developer.relayr.io/documents/HowTos/iBeacons">instructions on this page</a>. </p>

<h3> The Master Module comes with a battery - Does it have to be connected to a power supply at all times?</h3>

<p>We've discovered that due to a bug in the firmware, the battery charger of the Master Module, which also serves as a voltage regulator, enters an erratic state every ~4 hours, when not connected to a power supply - which eventually results in it disconnecting the WunderBar and breaking off any data transmission. This is why we are currently shipping the Master Modules with a battery, which will prevent it from getting to this state.</p>

<h3> Can I send audio to the relayr cloud, using the noise-level sensor?</h3>

<p>Our noise sensor is unable to send audio to the cloud as it is <em>not</em> a microphone. The output of this sensor is the relative level of noise at the location measured </p>

</div>

<h2 class="collapseHeader"> External Devices </h2>
<div class="collapse">

<h3> What are External Devices?</h3>
<p>By 'External Device' we refer to any device, apart from the relayr WunderBar, which can be integrated with the relayr platform.</p>


<h3> How do I add an external device?</h3>
<p> In order to add a new device, follow the instructions below
<list>
<li>In the Developer Dashboard, access the Devices menu> Add Device</li>
<li>Give your device a name and select the type of device you would like to add</li>
<li>Once you click Next a popup will be prompted for you to authorize the relayr app to access the devices under the account you've chosen.</li>
<li>Once the relayr deshboard is authorized to access your external account, you will be shown a list of your available devices.</li>
<li>Select the device you would like to add</li>
<li>The device selected will be added under 'My Devices'. You could now see live data from it and send commands to it.</li>
<li>To disconnect the device from the relayr Developer Dashboard, simply click the delete button on the top menu of the device.</li>
</p>

</div>

 