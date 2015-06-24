<h1>relayr for C# and .NET Developers</h1>
<p>We have some C# goodies just for you. We've created an easy to use <https://developer.relayr.io/documents/CSharp/Csharp" target="_self"><strong>C# SDK</strong></a> with the basic relayr related functionalities to get you started with your very first relayr C# Application. </p>
<p>The SDK allows you to access your WunderBar sensor data and the relayr Cloud Platform functionality from, you guessed it, apps built in C#!</p>
<p>Because it's built as a portable class library, you'll be able to use the same SDK across Windows 8.1, Windows Phone 8.1, and Windows Desktop Apps.</p>
<h2>Getting Started</h2>
<p>The C# SDK has two main functions: </p>
<ol>
<li>Programatically exposing the relayr API. This is achieved through the HttpManager class.</li>
<li>Allowing applications to subscribe to the streams of data published by sensor modules and other connected devices. The Transmitter and Device classes handle this functionality. You will need security strings obtained through the HttpManager to subscribe to data topics.</li>
</ol>
<p>To include the C# library simply download it from our <a href="https://github.com/relayr/csharp-sdk">C# Github repository </a> and add it as a reference to your own C# project.</p>
<h3>Adding the <em>M2MQTT</em> and the <em>Newtonsoft.Json</em> references</h3>
<p>Before you can build a project including the C# SDK, you'll need to add two references under {<em>Your project name</em>} &gt; <em>References</em></p>
<p>Simply navigate to the above location, <em>right click &gt; Add Reference &gt; Browse</em>
The two DLLs, <strong><em>M2MQTT</em></strong> and <strong><em>Newtonsoft.Json</em></strong> are actually included in the C# repository under <em>bin &gt; Debug</em> so you can simply reference their location.</p>
<h3>Getting an OAuth token</h3>
<p>The HttpManager requires an OAuth token to authorize your HTTP requests. You can obtain a token in two manners:</p>
<p>Via an OAuth authentication flow: Due to the platform-specific nature of performing this task, the C# SDK does not have the ability to execute this process for you. If you choose to get a token using this method, you will need to implement it yourself. Documentation on how the relayr OAuth implementation can be found on our <a href="https://developer.relayr.io/documents/relayrAPI/OAuthReference" target="_blank">OAuth Reference page</a>.</p>
<p>or</p>
<p>By generating one through the relayr Dashboard. On the <a href="https://developer.relayr.io/dashboard/apps/myApps" target="_blank">API Keys</a> page, create an app (or use an app previously created). 
Clicking the &quot;Generate Token&quot; button will create the required OAuth token. You can find detailed instructions for this procedure <a href="https://developer.relayr.io/documents/Browser/OAuthToken">here</a>. </p>
<p>This value can be hard-coded in your app, and allows you access to data from sensors and devices associated with your relayr account. This is extremely useful for prototyping and personal-use apps, however, it prevents you from being able to make your app widely available. You will need to use the first method of generating the token, if you wish to distribute your application.</p>
<h2>Using the SDK</h2>
<p>Below you will find the basic functions of the SDK. These will allow you to fetch a user, fetch the transmitter entities associated with the user as well as subscribing to data arriving from the different sensors.</p>
<h3>Setting the OAuth Token</h3>
<p>Once you've obtained your OAuth token using either of the methods described above, pass it to the relayr class in the following manner:</p>
<pre><code>    // Set the OAuth token
    Relayr.OauthToken = &quot;YOUR OAUTH TOKEN HERE&quot;;
</code></pre>

<h3>Retrieving Transmitters and Connecting to the MQTT Broker</h3>
<p>Next, you'll need to fetch a list of your transmitters (WunderBars) from the relayr API, using the <code>GetTransmitters()</code> function. 
You will use the ID of the transmitter to connect to the MQTT broker. Connections to the broker are performed on a per-Transmitter basis. You can have multiple connections simultaneously, one for each WunderBar. If you have multiple transmitters (Multiple WunderBars) you can choose an Id for each in order to tell them apart. This Id will be passes as <code>CLIENT ID OF YOUR CHOICE</code> below.</p>
<pre><code>    // Get a list of transmitters and connect to the MQTT broker with the first one in the list
    List&lt;dynamic&gt; transmitters = await Relayr.GetTransmittersAsync();
    Transmitter transmitter = Relayr.ConnectToBroker(transmitters[0], &quot;CLIENT ID OF YOUR CHOICE&quot;);
</code></pre>

<h3>Retrieving a List of Sensors and Subscribing to Data</h3>
<p>Finally, fetch a list of Devices (sensors) associated with a specific Transmitter by calling the <code>GetDevices()</code> function, in the Transmitter object instance. 
Once you have a sensor, you'll subscribe to the data being published by that sensor and define an event handler where you can choose how to handle the data received.</p>
<p>In the example below, we've subscribed to the first device on the list of associated devices.</p>
<pre><code>    // Get a list of devices associated with the transmitter, subscribe to data
    // From the first device on the list
    List&lt;dynamic&gt; devices = await transmitter.GetDevicesAsync();
    Device device = await transmitter.SubscribeToDeviceDataAsync(devices[0]);
    device.PublishedDataReceived += device_PublishedDataReceived;

// Create Handler for the the sensor's data published event
void device_PublishedDataReceived(object sender, PublishedDataReceivedEventArgs args)
{
      // Do something with the data here. Data is contained inside args
}
</code></pre>

<h2>Example: Sensor Channel Subscription Flow</h2>
<p>The example below is a typical use of the SDK, allowing you to subscribe to data arriving from a sensor.</p>
<pre><code>    // Set the OAuth token
    Relayr.OauthToken = &quot;YOUR OAUTH TOKEN HERE&quot;;

    // Get a list of transmitters and connect to the MQTT broker with the first one in the list
    List&lt;dynamic&gt; transmitters = await Relayr.GetTransmittersAsync();
    Transmitter transmitter = Relayr.ConnectToBroker(transmitters[0], &quot;CLIENT ID OF YOUR CHOICE&quot;);

    // Get a list of devices associated with the transmitter, subscribe to data
    // From the first device on the list
    List&lt;dynamic&gt; devices = await transmitter.GetDevicesAsync();
    Device device = await transmitter.SubscribeToDeviceDataAsync(devices[0]);
    device.PublishedDataReceived += device_PublishedDataReceived;

// Create Handler for the the sensor's data published event
void device_PublishedDataReceived(object sender, PublishedDataReceivedEventArgs args)
{
      // Do something with the data here. Data is contained inside args
}
</code></pre>