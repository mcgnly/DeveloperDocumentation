# The relayr API 

The relayr API provides programmatic access to all entities on the relayr platform. These include users, devices, device models, publishers, transmitters and apps. 
The relayr iOS Framework and Android SDK provide wrapped versions of the API calls, therefore, developers using the latter do not need to use the bare API endpoints. 
If you are planning on using other frameworks, we recommend that you start by getting to know the available API endpoints. 

**Our API calls are currently available for download to Postman. Simply log in to the Postman console and import the calls from the following link: [https://www.getpostman.com/collections/65008c7fc1137e6664b5](https://www.getpostman.com/collections/65008c7fc1137e6664b5)**

## API Entities

### Users

The first basic entity in the relayr platform is the **user**. 
Every user registers with an email address, a respective name and password and 
is assigned a unique _userId_. A user can be both an application owner (a publisher) and an end
user.
A user is required in order to add other entities to the relayr platform.

### Publishers

The second basic entity in the relayr platform is the **publisher**. 
Each user can choose to have the option to publish apps on the relayr platform 
and they are then assigned a _publisherId_. 

### Apps

An app is the third basic entity in the relayr platform. 
The relayr platform relates to apps in two manners: _Publisher Apps_ and 
_User Apps_. 

**Publisher apps** are apps which are purchasable on an app store
and are owned by a publisher.

**User apps** are apps which have been approved to the data of an end user. 
This approval has been granted by the user.

### Devices

The fourth basic entity in the relayr platform.
A device is any external entity capable of producing measurements 
and sending them to the relayr platform or one which is capable of receiving
information from the relayr platform. 
Examples would be a thermometer, a gyroscope or an infrared sensor.

### Device Models

A device model is an entity which holds the following information about a device:

+ Name
+ Manufacturer  
+ List of readings provided along with the minimum and maximum values and the units in which these are measured.

A _reading_ is the information gathered 
by the device.

### Transmitters

Transmitters are the fifth basic entity on the relayr platform. A transmitter contrary
to a device does not gather data but is only used to relay the data from the devices
to the relayr platform. The transmitter is also used to authenticate the different
devices that transmit data via it.