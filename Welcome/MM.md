# The WunderBar Master Module

The Master Module is the master mind behind the WunderBar. It is the only module capable of connecting to Wifi and is responsible for delivering data from the sensors to the cloud and from the cloud to the sensors. We'd like to give you a some insight into the relayr Master Module. 


The Master Module consists of three main parts, each responsible for a different aspect of its overall functionality: 

<img src="assets/Master.png" class="center">

### The Gainspan module 

Incorporating the <a href="http://www.gainspan.com/gs1500m">GS1500m chip,</a> the Gainspan module is responsible for WiFi communication. It initiates the communication with the relayr cloud, utilizing the MQTT/SSL protocol and is responsible for the connection with the MQTT broker.


### The Kinetis module 

Incorporating the <a href="http://www.arm.com/products/processors/cortex-m/cortex-m4-processor.php">ARM Cortex M4</a>, the <a href="http://www.freescale.com/webapp/sps/site/overview.jsp?code=KINETIS_K_SERIES" target="_blank"> Kinetis K Series </a>, manufactured by Freescale, is the core chip on the Master Module.
It stores and maintains all the authorization keys are secrets for the communication with the <a href="https://developer.relayr.io/documents/Welcome/Platform">Cloud Platform </a> and the communication with the Sensor Modules. 
It includes a calculating unit which converts the <a href="https://developer.relayr.io/documents/Welcome/Sensors">raw readings data arriving from the sensor modules </a> into meaningful readings. It also 'translates" BLE into JSON/MQTT - enabling both the SDKs and the Cloud Platform to communicate with the sensor modules.
It communicates with the Gainspan module instructing it when to connect to WiFi and with which credentials. 
Lastly, it runs the Bootloader application which manages the firmware upgrade. 


### The Nordic Central module

Incorporating the <a href="https://www.nordicsemi.com/eng/Products/Bluetooth-Smart-Bluetooth-low-energy/nRF51822"> nRF51822 </a> chip this module is responsible for all Bluetooth Low Energy communication including scanning for available BLE devices and connecting to them as well as scanning for the OnBoarding service during the onboarding process.  

### The Master Module LEDs 

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

