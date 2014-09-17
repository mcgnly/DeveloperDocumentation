# Using the WunderBar Sensor Modules as iBeacons

We are very excited to introduce another way of using the WunderBar Sensor Modules.
Every sensor could be used as an iBeacon compatible device in a few small steps. What you will need to do is simply replace the existing firmware on the module with the iBecon firmware and you are good to go. Below you will find a step by step tutorial for turning your sensors into iBeacons but first, here are a few words about iBeacons.

## A Few Words about iBeacons

an iBeacon is a low cost, low energy consuming transmitter, which, when in close proximity to a smart phone, could enable the phone to perform actions such as send notifications and enable point of sale payments - i.e. payments which do not require a customer to reach for their wallet.

Utilizing <a href="http://en.wikipedia.org/wiki/Bluetooth_low_energy" target="_blank">Bluetooth Low Energy</a>, iBeacons transmit a <a href="http://en.wikipedia.org/wiki/Universally_unique_identifier" target="_blank">Universally Unique Identifier</a> which can then be picked up by a compatible app or operating system to either determine their exact location or to trigger an action such as push a notification. 

## The iBeacon Packet - Description

An iBeacon's advertising packet consists of 4 components:

### Universally Unique Identifier (UUID): 
The UUID comes in a 16 byte string format that distinguishes a company's beacons from others. The relayr iBeacon firmware uses the UUID **289A1DC3-28AB-03F9-842A-00805F9B34FB**

### Major value: 
The Major value is a 2 byte string and is used to specify a beacon within a group. This number is a random string taken from the chip's internal serial number.  For example, if the Beacons are used in a department store with different branches, the Major value could denote the branch identifier.

### Minor value: 
The Minor value is a 2 byte string and is used to identify a specific beacon. This number is a random string taken from the chip's internal serial number. In our department store example, this number could identify the specific beacon in an area of the store.

### Transmit Power: 
Is a calibrated value that indicates how strong the received signal (RSSI) is at a certain distance. This helps the scanning application estimate the distance to the Beacon. 

**Note**: The accuracy of the distance estimation might be different for different devices, the signal received on an iPhone 5S could be from the one received on a Samsung Galaxy tablet for example. The approximation will be mostly affected by objects between the beacon and the scanning device such as walls, furniture, and by the presence of human beings.

## Setting up the Sensor Modules as iBeacons

Below is a step by step tutorial of the way the modules could be turned into iBeacons. The applications which are used by Android and iOS are different, however, the overall process is identical. 

### For Android Users

1. Download the relayr iBeacon Firmware from <a href="">this link</a>
2. In case you have not downloaded the firmware to your phone, copy it to your phone.
3. Install the Nordic DFU (Device Firmware Update) application. The application compatible with Android is the **nRF Master Control Panel (BLE)**, which can be downloaded from the <a href="https://play.google.com/store/apps/details?id=no.nordicsemi.android.mcp">Play Store</a>
4. Put the Sensor Module which you would like to use as an iBeacon in *DFU Mode*. You can do this by pressing the button on the module and holding for 3 seconds. The LED will be constantly on when the DFU mode is activated.
5. Connect to the Module and open the DFU pane.
6. Press to scan. The module should then appear in the list of available BLE devices. The Device Name is "*Dfu{module_name}*". For example: DfuMIC for the Noise Level Sensor:

<img src="assets/first.jpg" class="center" width=300px>

7. The app will detect the DFU service and the DFU button will appear. 

<img src="assets/second.jpg" class="center" width=300px>

8. Press the button and select the Application option. Then select the firmware file downloaded in step 2 (relayrBeacon.bin). 

<img src="assets/third.jpg" class="center" width=300px>


9. The module will now be broadcasting the Beacon packet. You can verify this using any iBeacon compatible application.

### For iOS Users 

1. Download the relayr iBeacon Firmware from <a href="">this link</a>
2. In case you have not downloaded the firmware to your phone, copy it to your phone.
3. Install the Nordic DFU (Device Firmware Update) application. The application compatible with Android is the **nRF Toolbox**, which can be downloaded from the <a href="https://itunes.apple.com/us/app/nrf-toolbox/id820906058?mt=8">Apple Store</a>
4. Put the Sensor Module which you would like to use as an iBeacon in *DFU Mode*. You can do this by pressing the button on the module and holding for 3 seconds. The LED will be constantly on when the DFU mode is activated.
5. Connect to the Module and open the DFU pane.
6. In the nRF Toolbox, open the DFU Utility. 

<img src="assets/fourth.png" class="center" width=300px>

7. Select "Application" in the Select File Type setting.
8. Click "Select Device" - the module should appear in the list of available BLE devices.
9. Select "Upload" to upload the firmware onto the sensor module

<img src="assets/fifth.png" class="center" width=300px>