# Using the WunderBar Sensor Modules as iBeacons

We are very excited to introduce another way of using the WunderBar Sensor Modules.
Every sensor could be used as an iBeacon compatible device in a few small steps. What you will need to do is simply replace the existing firmware on the module with the iBeacon firmware and you are good to go. Below you will find a step by step tutorial for turning your sensors into iBeacons but first, here are a few words about iBeacons.

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

## Setting up Sensor Modules as iBeacons

Below are instructions for turning the sensor modules into iBeacons. 

1. Download the relayr iBeacon Firmware from <a href="https://s3-eu-west-1.amazonaws.com/relayr-firmware/BLEModulesFirmware-20140910.zip">this link</a>. The relevant file is the **iBeaconRelayr600ms.hex** file, located under *\BLEModulesFirmware-20140910/BLEModulesFirmware*
2. In case you have not downloaded the firmware to your phone, copy it to your phone.
3. Follow our step by step tutorial for [flashing a sensor module and replacing its firmware](https://developer.relayr.io/documents/Hardware/Flashing). The applications which are used by Android and iOS are different, however, the overall process is identical. 

4. The module will now be broadcasting the Beacon packet. You can verify this using any iBeacon compatible application.


That is it! You are good to go. Start using our modules as iBeacons and don't forget, if you change your mind and decide that you would like to revert to the original sensor functionality- you could do so by flashing the modules and re-installing the original sensor firmware. The original firmware for each of the sensor modules is a part of the zip file downloaded in the above section. 
