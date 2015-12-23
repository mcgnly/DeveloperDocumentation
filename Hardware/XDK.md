# CONNECTING THE BOSCH XDK TO THE RELAYR CLOUD

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk1.png)

## Overview


The [Bosch XDK](http://xdk.bosch-connectivity.com/) is a prototyping platform from Bosch Connected Devices and Solutions for the Internet of Things - it is packed with state-of-the-art sensors and comes equipped with wireless connectivity via wifi and bluetooth, as well as a rechargeable Li-ion battery and microSD slot. Because it was developed to be essentially plug-and-play, it is easy to quickly create prototypes of systems to monitor environmental conditions such as temperature, humidity, ambient light, movement, magnetic fields, and sound. 

Currently these  devices are deployed in the Cisco Open Berlin Innovation Center to measure temperature, light, and noise levels, though other possibilities  include monitoring air quality in an office building, noise levels in a factory, or conditions leading to decay or corrosion in a warehouse such as humidity and temperature. When combined with the relayr dashboard for visualizing the sensor outputs, possibilities for connected solutions for your company are as great as your imagination!

## Creating your Bosch XDK on the relayr cloud

Start by downloading [GCC ARM Embedded](https://launchpad.net/gcc-arm-embedded/4.9/4.9-2015-q3-update), [Python 2](https://www.python.org/downloads/), the text editor of your choice (I used [Atom](https://atom.io/docs/v0.191.0/getting-started-installing-atom)) and the [XDK Workbench](http://xdk.bosch-connectivity.com/en/web/bcdscommunitygb/forum/-/category/using-xdk-downloads/10796-c), which we will need for the Makefiles included in it. If you are working on a Mac or Linux, you will also need the [Segger JLink debugger](https://www.segger.com/jlink-software.html) and debugger board hardware (these are not needed if you are on Windows). 

The Segger JLink utility and ARM GCC toolchain are needed to compile and flash XDK firmware on Mac and Linux (on Windows it is bundled with XDK Workbench and flashing is done via USB). Make sure that arm-none-eabi-gcc and JLinkExe binaries are accessible from command line. You might need to append paths to those tools to your shell PATH environment variable.

Now you can start setting up your device on the [relayr dashboard](https://developer.relayr.io/). Sign in or make an account, and go to “Your Devices”. Add a new device, and select “Create my Own Device”. 

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk2.png)

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk3.png)

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk4.png)

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk5.png)

Give your device a name, and click  “Start Prototyping”.

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk6.png)

You will be taken to this screen - the important part here  is the “Prototype Credentials” box. Save the text here, because you will need these credentials later! 

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk7.png)

You can now scroll down below the “Example Usage” code and click “Select a Model”. 

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk8.png)

Select “Create new Model” and give it a name, then continue with “Start Modeling”.

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk9.png)

Click “Save”, then “Finish” in the relayr dashboard. 

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk10.png)

##Configuring your XDK

Next, head over to the [XDK on Relayr repo](#) and clone it. Open it in your text editor, because we need to fill out a few specifics. 

First, navigate to xdk110/make/application.mk, to add some specifications for your system. The paths for Windows and Linux are already filled out, but you should double check to make sure they are correct and that it points to directory of your arm-gcc installation. For Mac you should add this directory yourself. 

Within this same file, change the variables “SHELLNAMES” and “SH” to your shell. Double check that the correct shell is named here, since yours might be different that the default option listed here. 

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk11.png)

If you are not using OSX, change TOOLDIR to your OS. On Windows, uncomment the block of code below it , and comment out the second entry of SH, as well as the second entry for FLASH. 

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk12.png)

Next, navigate to xdk110/app/relayrxdk/paho/MQTTClient/MQTTXDK.h and specify the name and password of your wifi network, inside double-quotes.

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk13.png)

Now, head to xdk110/app/relayrxdk/src/ and create a new file named “credentials.json”.  Paste in the “Prototype Credentials” you saved during device setup on the relayr dashboard. Add double-quotes around user, password, clientID, and topic, as shown above. 

Make sure all of your changes in the text editor are saved, then in the terminal, navigate to the /xdk/xdk110/app/relayrxdk/make directory if you aren’t there already. 

Plug in your Bosch XDK hardware to your computer and make sure it is powered on -   green LED will be on. If yours isn’t, make sure the switch is on and both the cable for  flashing/programming/debugging (and if on a Linux or Mac, the JLink debugger) and the USB cable  are plugged in. 

Once everything is connected, execute ‘make’ and ‘make flash’  from the terminal, and you should see live readings from the sensors! 

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk14.png)

Congratulations! You successfully connected your XDK to the relayr cloud! 

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk15.png)

You can access the Device Activity log from this icon in the top right corner of your dashboard, which will also provide you with a command line to control your device.

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk16.png)

Try sending some commands to your XDK - for example you can turn the LEDs on and off by typing into the command line on the Dashboard.

	{"path" : "led", "command" : "<ledcolor>", "value" : "<0, 1, or 2>"}

* the options for ledcolor are : "red", "yellow", or "orange"
* the option for values are: 0 for off, 1 for on, and 2 to toggle

![title image](https://github.com/DeveloperDocumentation/Hardware/assets/BoschXDKtutorialImages/xdk17.png)

Have fun creating, and feel free to leave a comment here or in the forums with questions or ideas for projects!
