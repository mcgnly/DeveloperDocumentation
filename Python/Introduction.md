# relayr for Pythonists

Welcome to the relayr Python Library Documentation. [The repository](https://github.com/relayr/python-sdk) provides Python developers with  programmatic access points to the relayr platform.

These include access to the relayr cloud via the [relayr API](https://developer.relayr.io/documents/relayrAPI/Introduction) as well as direct connection to the relayr WunderBar sensors, via Bluetooth Low Energy (using [BlueZ](http://www.bluez.org/) on Linux). 


## Installation

You can install the library using one of the following methods, using Pip: 

1. You can download the very latest version of the repository from GitHub:

    	pip install git+https://github.com/relayr/python-sdk

2. You can use the following to install the package from the [Python Package Index](https://pypi.python.org/pypi/relayr/) 

    
		pip install relayr


## Usage Examples


### Switching a device's LED on/off:


		from relayr import Client
		c = Client(token='<my_access_token>')
    	d = c.get_device(id='<my_device_id>')
    	d.switch_led_on(True)

### Receiving a 10 second data stream from a device:

Receive a 10 second data stream, from one of your WunderBar sensors (device). In the example below the device does not have to be a public one in order to be used. You can obtain your device IDs from the relayr Dashboard [My Devices section](https://developer.relayr.io/dashboard/devices):

		import time
	    from relayr import Client
	    from relayr.dataconnection import MqttStream
	    c = Client(token='<my_access_token>')
	    dev = c.get_device(id='<my_device_id>')
	    def mqtt_callback(topic, payload):
	        print('%s %s' % (topic, payload))
	    stream = MqttStream(mqtt_callback, [dev])
	    stream.start()
	    time.sleep(10)
	    stream.stop()

**PLEASE NOTE**: Receiving data via [MQTT](http://mqtt.org/) will work only for Python versions 2.7 and above due to limited support in `paho-mqtt` for TLS in Python 2.6.

## Full Documentation Reference

The full reference of the package could be obtained in one of the following methods: 

1. The documentation can be found in the ***Docs*** sub directory in the Github repository and it can be rendered in various formats using a helper script named [`make_docs.sh`](https://github.com/relayr/python-sdk/blob/master/make_docs.sh).


2. You can also access the full reference on the [Read the Docs](http://relayr.readthedocs.org/) website.


