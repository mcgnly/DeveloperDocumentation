<h1>relayr - Raspberry Pi - Demo</h1>
<p>This is a simple demo project that shows how to connect a simple
sensor attached to a <a href="https://www.raspberrypi.org">Raspberry Pi</a> to the <a href="https://relayr.io">relayr</a> cloud. It uses
a digital temperature sensor, namely the very popular <a href="http://www.maximintegrated.com/en/products/analog/sensors-and-sensor-interface/DS18B20.html">DS18B20</a>, and
two simple <a href="https://www.python.org">Python</a> scripts, one to send the sensor data to the cloud,
and the other to receive it from there to be used for some other
application. The entire sample code is provided in a
<a href="https://github.com/relayr/relayr-raspberry-pi">GitHub repository</a>.</p>
<a name="user-content-connect-a-sensor-to-the-raspberry-pi"></a>
<h2><a id="user-content-connect-a-sensor-to-the-raspberry-pi" class="anchor" href="#connect-a-sensor-to-the-raspberry-pi" aria-hidden="true"><span class="octicon octicon-link"></span></a>Connect a sensor to the Raspberry Pi</h2>
<p>First you connect the sensor to your Raspberry Pi. You can find
a <a href="http://www.reuk.co.uk/DS18B20-Temperature-Sensor-with-Raspberry-Pi.htm">detailed description</a> on how to connect the sensor (including a
4.7 K-Ohm resistor) like the one on the <a href="http://www.reuk.co.uk">Renewable Energy Website</a>
in the UK. It describes how to connect the sensor (using the <a href="https://en.wikipedia.org/wiki/1-Wire">1wire</a>
bus) with three GPIO pins (power, ground and the actual data pin).</p>
<div>

<p>Temperature sensor connected to a Raspberry Pi 2 B (the USB dongle
is for connecting to the Wifi).</p>
</div>
<p>After that you must ensure the 1wire communication device kernel
module is loaded. The procedure for doing that is slightly different
between Raspberry Pi versions before and after January 2015 when kernel
3.18.8 was included in <a href="https://www.raspbian.org">Raspbian</a>, the most widely used Linux
distribution for the Raspberry Pi. In the recent updates you have to
modify the file <code>/boot/config.txt</code> as described here:</p>
<div class="highlight highlight-source-shell"><pre><span class="pl-c"># with a pre-3.18.8 kernel:</span>
pi@raspberrypi <span class="pl-k">~</span> $ sudo modprobe w1-gpio <span class="pl-k">&amp;&amp;</span> sudo modprobe w1_therm

<span class="pl-c"># else:</span>
pi@raspberrypi <span class="pl-k">~</span> $ uname -a
Linux raspberrypi 3.18.11-v7+ <span class="pl-c">#781 SMP PREEMPT Tue Apr 21 18:07:59 BST 2015 armv7l GNU/Linux</span>
pi@raspberrypi <span class="pl-k">~</span> $ sudo nano /boot/config.txt
<span class="pl-c"># add this line at the bottom (and then reboot):</span>
<span class="pl-c"># dtoverlay=w1-gpio</span></pre></div>
<p>Now you can look for respective 1wire devices in your file system. Each
<a href="http://www.maximintegrated.com/en/products/analog/sensors-and-sensor-interface/DS18B20.html">DS18B20</a> sensor has a unique ID that appears in this devices directory,
in our case <code>28-000004a365ef</code>. The temperature value is accessible
under the respective path ending in <code>w1_slave</code> in the last line
right next to the string <code>t=</code> (multiplied by 1000):</p>
<div class="highlight highlight-source-shell"><pre>pi@raspberrypi <span class="pl-k">~</span> $ ls -l /sys/bus/w1/devices
total 0
lrwxrwxrwx 1 root root 0 Jun  9 17:08 28-000004a365ef -<span class="pl-k">&gt;</span> ../../../devices/w1_bus_master1/28-000004a365ef
lrwxrwxrwx 1 root root 0 Jun  9 17:08 w1_bus_master1 -<span class="pl-k">&gt;</span> ../../../devices/w1_bus_master1

pi@raspberrypi <span class="pl-k">~</span> $ cat /sys/bus/w1/devices/28-000004a365ef/w1_slave
93 01 4b 46 7f ff 0d 10 32 <span class="pl-c1">:</span> crc=32 YES
93 01 4b 46 7f ff 0d 10 32 t=25187</pre></div>
<p>So the current temperature is 25.187 degrees Celsius! The next section
shows how to read this information in a more reusable way so it
can be communicated easily elsewhere.</p>
<a name="user-content-read-the-sensor-data"></a>
<h2><a id="user-content-read-the-sensor-data" class="anchor" href="#read-the-sensor-data" aria-hidden="true"><span class="octicon octicon-link"></span></a>Read the sensor data</h2>
<p>Once you know the unique ID of your <a href="http://www.maximintegrated.com/en/products/analog/sensors-and-sensor-interface/DS18B20.html">DS18B20</a>, you can access the
sensor data in a more reusable way with a Python function like this:</p>
<div class="highlight highlight-source-python"><pre><span class="pl-k">def</span> <span class="pl-en">read_temperature</span>(<span class="pl-smi">device_id</span>):
    <span class="pl-s"><span class="pl-pds">"</span>Read float temperature value from 1wire device DS18B20.<span class="pl-pds">"</span></span>
    <span class="pl-k">with</span> <span class="pl-c1">open</span>(<span class="pl-s"><span class="pl-pds">'</span>/sys/bus/w1/devices/<span class="pl-c1">%s</span>/w1_slave<span class="pl-pds">'</span></span> <span class="pl-k">%</span> device_id) <span class="pl-k">as</span> f:
        text <span class="pl-k">=</span> f.read().strip()
        fragments <span class="pl-k">=</span> text.split()
        <span class="pl-k">return</span> <span class="pl-c1">float</span>(fragments[<span class="pl-k">-</span><span class="pl-c1">1</span>][<span class="pl-c1">2</span>:]) <span class="pl-k">/</span> <span class="pl-c1">1000.</span></pre></div>
<p>You can test this by writing a short loop to read and display the
temperature while you try to change the ambiant temperature around
the sensor:</p>
<div class="highlight highlight-source-python"><pre><span class="pl-k">&gt;&gt;&gt;</span> <span class="pl-k">import</span> time
<span class="pl-k">&gt;&gt;&gt;</span> <span class="pl-k">while</span> <span class="pl-c1">True</span>:
...     <span class="pl-k">print</span>(read_temperature(<span class="pl-s"><span class="pl-pds">'</span>28-000004a365ef<span class="pl-pds">'</span></span>))
...     time.sleep(<span class="pl-c1">1</span>)
...
<span class="pl-c1">25.312</span>
<span class="pl-c1">25.312</span>
<span class="pl-c1">25.375</span>
<span class="pl-c1">25.75</span>
<span class="pl-c1">26.937</span>
<span class="pl-c1">28.75</span>
<span class="pl-c1">30.437</span>
<span class="pl-c1">29.875</span>
<span class="pl-c1">26.562</span>
<span class="pl-c1">25.875</span></pre></div>
<p>Now that the sensor is working and delivers data it's time to push
that data into the relayr cloud as shown in the next section.</p>
<a name="user-content-create-a-device-prototype-in-the-relayr-dashboard"></a>
<h2><a id="user-content-create-a-device-prototype-in-the-relayr-dashboard" class="anchor" href="#create-a-device-prototype-in-the-relayr-dashboard" aria-hidden="true"><span class="octicon octicon-link"></span></a>Create a device prototype in the relayr dashboard</h2>
<p>If you don't have a <a href="https://developer.relayr.io">relayr developer</a> account, please create one,
first. Once you have an account, you create a sensor prototype simply
by going on your <a href="https://developer.relayr.io/dashboard/devices">relayr devices page</a> and moving your mouse pointer
on the big plus button in the lower right corner. In the pop-up
menu which then shows up you click on "Add prototype".</p>
<p>On the next page you create a <a href="https://developer.relayr.io/dashboard/prototype">relayr device prototype</a> by first
entering a name for your device. Clicking on "Add prototype" then
will show a page with some credentials which you should save as they
are necessary for connecting your sensor, later. These credentials
come as a JSON dictionary like this (don't use those shown here, as
they are made up):</p>
<div class="highlight highlight-source-python"><pre>{
  <span class="pl-s"><span class="pl-pds">"</span>user<span class="pl-pds">"</span></span>:     <span class="pl-s"><span class="pl-pds">"</span>565738d3-29ef-442d-b055-debb1a1be13c<span class="pl-pds">"</span></span>,
  <span class="pl-s"><span class="pl-pds">"</span>password<span class="pl-pds">"</span></span>: <span class="pl-s"><span class="pl-pds">"</span>442SsprjRXbY<span class="pl-pds">"</span></span>,
  <span class="pl-s"><span class="pl-pds">"</span>clientId<span class="pl-pds">"</span></span>: <span class="pl-s"><span class="pl-pds">"</span>TVlc51xjvQxywVd67GhvhPA<span class="pl-pds">"</span></span>,
  <span class="pl-s"><span class="pl-pds">"</span>topic<span class="pl-pds">"</span></span>:    <span class="pl-s"><span class="pl-pds">"</span>/v1/565738d3-29ef-442d-b055-debb1a1be13c/<span class="pl-pds">"</span></span>
}</pre></div>
<a name="user-content-publish-your-sensor-data-into-the-relayr-cloud"></a>
<h2><a id="user-content-publish-your-sensor-data-into-the-relayr-cloud" class="anchor" href="#publish-your-sensor-data-into-the-relayr-cloud" aria-hidden="true"><span class="octicon octicon-link"></span></a>Publish your sensor data into the relayr cloud</h2>
<p>You can publish your data using <a href="http://mqtt.org">MQTT</a> (a protocol for communicating
messages from machine to machine) which needs to be installed on
your Raspberry Pi if not available, yet. The <code>paho-mqtt</code> package
provides MQTT support for Python and can be easily installed as a
Python package with <code>pip</code> like this (install <code>pip</code> first if you
don't have it, yet):</p>
<div class="highlight highlight-source-shell"><pre>pi@raspberrypi <span class="pl-k">~</span> $ sudo apt-get install python-pip
pi@raspberrypi <span class="pl-k">~</span> $ sudo pip install paho-mqtt==1.1</pre></div>
<p>You have successfully installed it if you can run this statement in
Python without any error: <code>import paho</code>.</p>
<p>Then you copy the sample Python snippet from the dashboard prototype
page that you've seen when creating a prototype. The <a href="https://github.com/relayr/relayr-raspberry-pi">GitHub repository</a>
contains a file named <code>publish_data_mqtt.py</code> which is a slightly expanded
version of that code snippet from the dashboard. If you start it on your
Raspberry Pi (first make sure, you use your own credentials!) it will run
in an endless loop, reading temperature values and publishing them one
per second to the relayr cloud.</p>
<a name="user-content-watch-your-sensor-data-on-the-relayr-dashboard"></a>
<h2><a id="user-content-watch-your-sensor-data-on-the-relayr-dashboard" class="anchor" href="#watch-your-sensor-data-on-the-relayr-dashboard" aria-hidden="true"><span class="octicon octicon-link"></span></a>Watch your sensor data on the relayr dashboard</h2>
<p>As you push your data into the relayr cloud you can see the live values
as they change in the relayr <a href="https://developer.relayr.io/dashboard/">dashboard</a>. In the following screenshot the
device prototype was named Shiny:</p>
<div>
<p>Widget showing temperature of your device prototype in the relayr
dashboard.</p>
</div>
<a name="user-content-fetch-your-sensor-data-from-the-relayr-cloud"></a>
<h2><a id="user-content-fetch-your-sensor-data-from-the-relayr-cloud" class="anchor" href="#fetch-your-sensor-data-from-the-relayr-cloud" aria-hidden="true"><span class="octicon octicon-link"></span></a>Fetch your sensor data from the relayr cloud</h2>
<p>Watching your data in the dashboard as it changes is great, but at some
moment you'll want to fetch it for really doing something with it. For
that purpose you can access your data via MQTT again by writing a simple
script like the one named <code>fetch_data_mqtt.py</code> in the <a href="https://github.com/relayr/relayr-raspberry-pi">GitHub repository</a>.
If you just run that script it will show the live MQTT messages containing
the data values as received:</p>
<div class="highlight highlight-source-shell"><pre>pi@raspberrypi <span class="pl-k">~</span> $ python fetch_data_mqtt.py
Connecting to mqtt server.
Connected.
Message received: {<span class="pl-s"><span class="pl-pds">"</span>meaning<span class="pl-pds">"</span></span>: <span class="pl-s"><span class="pl-pds">"</span>temperature<span class="pl-pds">"</span></span>, <span class="pl-s"><span class="pl-pds">"</span>value<span class="pl-pds">"</span></span>: 25.187}
Message received: {<span class="pl-s"><span class="pl-pds">"</span>meaning<span class="pl-pds">"</span></span>: <span class="pl-s"><span class="pl-pds">"</span>temperature<span class="pl-pds">"</span></span>, <span class="pl-s"><span class="pl-pds">"</span>value<span class="pl-pds">"</span></span>: 25.187}
Message received: {<span class="pl-s"><span class="pl-pds">"</span>meaning<span class="pl-pds">"</span></span>: <span class="pl-s"><span class="pl-pds">"</span>temperature<span class="pl-pds">"</span></span>, <span class="pl-s"><span class="pl-pds">"</span>value<span class="pl-pds">"</span></span>: 25.187}
Message received: {<span class="pl-s"><span class="pl-pds">"</span>meaning<span class="pl-pds">"</span></span>: <span class="pl-s"><span class="pl-pds">"</span>temperature<span class="pl-pds">"</span></span>, <span class="pl-s"><span class="pl-pds">"</span>value<span class="pl-pds">"</span></span>: 25.562}
Message received: {<span class="pl-s"><span class="pl-pds">"</span>meaning<span class="pl-pds">"</span></span>: <span class="pl-s"><span class="pl-pds">"</span>temperature<span class="pl-pds">"</span></span>, <span class="pl-s"><span class="pl-pds">"</span>value<span class="pl-pds">"</span></span>: 25.75}
Message received: {<span class="pl-s"><span class="pl-pds">"</span>meaning<span class="pl-pds">"</span></span>: <span class="pl-s"><span class="pl-pds">"</span>temperature<span class="pl-pds">"</span></span>, <span class="pl-s"><span class="pl-pds">"</span>value<span class="pl-pds">"</span></span>: 25.937}
Message received: {<span class="pl-s"><span class="pl-pds">"</span>meaning<span class="pl-pds">"</span></span>: <span class="pl-s"><span class="pl-pds">"</span>temperature<span class="pl-pds">"</span></span>, <span class="pl-s"><span class="pl-pds">"</span>value<span class="pl-pds">"</span></span>: 25.875}
^C^C</pre></div>
<p>If you use one of the relayr SDKs like the <a href="https://github.com/relayr/python-sdk">Python SDK</a> this will become
even more convenient, reducing the sample code to access your data
by more than half. The <a href="https://github.com/relayr/relayr-raspberry-pi">GitHub repository</a> contains a script named
<code>fetch_data_sdk.py</code> to show how to do that. If you are on a fresh
Raspberry Pi, make sure you update its Debian package list and install
some developer packages, before installing the newest relayr package
from GitHub like this:</p>
<div class="highlight highlight-source-shell"><pre>pi@raspberrypi <span class="pl-k">~</span> $ sudo apt-get update
pi@raspberrypi <span class="pl-k">~</span> $ sudo apt-get install python-dev libffi-dev libssl-dev
pi@raspberrypi <span class="pl-k">~</span> $ pip install git+https://github.com/relayr/python-sdk</pre></div>
<a name="user-content-wrap-up"></a>
<h2><a id="user-content-wrap-up" class="anchor" href="#wrap-up" aria-hidden="true"><span class="octicon octicon-link"></span></a>Wrap-Up</h2>
<p>In this simple demo project you've seen how you can easily connect a
simple temperature sensor to a <a href="https://www.raspberrypi.org">Raspberry Pi</a> and publish its data
from there into the <a href="https://relayr.io">relayr</a> cloud. There you can see it displayed live
in a widget on your relayr dashboard, or with just a few lines of code
you can fetch your data from the relayr cloud to be used in some
application. You can use MQTT to publish and receive the sensor data,
or one of the relayr SDKs, like the <a href="https://github.com/relayr/python-sdk">Python SDK</a>, for fetching the data
more conveniently.</p>
<p>Surely, you can use more exciting sensors and also publish
data values more complex than a single float, e.g. a list of three
floats representing some geospatial information. Whenever you provide
a <em>reading</em> known to the relayr dashboard it will show your data in
a nice widget. And you can also publish something even more involved,
like an object with deeper nesting levels. In that case the dashboard
will show a generic widget. It's up to you and your use case. The source
code on GitHub for the file <code>publish_data_mqtt.py</code> has some commented
lines of code to give you directions of how to do that.</p>