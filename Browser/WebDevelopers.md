<h1>relayr for Web Developers</h1>
<p>We don't want to teach you how to create a web app. We are quite positive that you know your way around web development tools. We've created an easy to use <strong>JavaScript micro SDK</strong> with the basic relayr related functionalities to get you started with your very first relayr Web Application.</p>
<p>Please note that this SDK is meant for web browser applications only. </p>
<h2>Implementing the relayr JavaScript SDK</h2>
<p>In order to start using the SDK all you would need to do is to include the <code>relayr-browser-sdk.min.js</code> file in your project:</p>
<pre><code>&lt;script src=&quot;https://developer.relayr.io/relayr-browser-sdk.min.js&quot;&gt;&lt;/script&gt;
</code></pre>

<p><strong>Download the <code>relayr-browser-sdk.min.js</code> file from our <a href="https://github.com/relayr/browser-sdk" target="_blank">JavaScript micro SDK repository</a></strong></p>
<hr />
<h2>Using the SDK</h2>
<h3>Initializing the SDK</h3>
<p>The following function Initializes the Relayr SDK. It receives two parameters: 
<code>redirectURI</code> and <code>appID</code></p>
<p><code>Appid</code>: The ID of your app as it appears in the Developer Dashboard <a href="https://developer.relayr.io/dashboard/apps/myApps">API-Keys Section</a>.<br/>
<code>redirectUri</code>: The redirectUri provided on App creation. You may use <em>http://localhost</em> for locally-hosted projects. </p>
<pre><code>var relayr = RELAYR.init({
  appId: &quot;YOUR_APP_ID&quot;,
  redirectUri:&quot;YOUR_REDIRECT_URL&quot;
});
</code></pre>

<hr />
<h3>Checking for the Existence of a Token</h3>
<p>This function will check for the existence of a token (stored on your browser). If it does not find a token it will redirect to a Login page and use your <code>redirectUri</code> to come back with the token.</p>
<p>The function will recognize the token and automatically call the method <code>success</code>.</p>
<p><strong><em>success</em></strong> is a method called when the login flow is completed. 
(it has an optional token return variable: <code>success: function(token){}</code>)</p>
<pre><code>relayr.login({
  success: function(token){

  }
});
</code></pre>

<hr />
<h3>Retrieving a List of Devices</h3>
<p>This function will return an array of the devices associated with your account.</p>
<p><strong>Note</strong>: This function should be called following a successful Login.</p>
<pre><code>relayr.devices().getAllDevices(function(devices){

});
</code></pre>

<hr />
<h3>Subscribing to a Device Channel and Receiving Data</h3>
<p>This function will subscribe the app to a device channel and start listening to your device. Data will be received into the <code>incomingData</code> method.
<strong>Note</strong>: This function needs to be called after a successful Login.</p>
<p>The function receives two variables:</p>
<p><code>deviceId</code>: The ID of the device to subscribe to.</p>
<p><code>incoingData</code>: This method is triggered each time the device sends data. </p>
<p><code>token</code>: (optional) you can pass a token variable you generated from the developer dashboard to subscribe to the device <em>without having to Log in</em>. </p>
<pre><code>relayr.devices().getDeviceData({
  deviceId: &quot;YOUR DEVICE ID&quot;, 
  incomingData: function(data){

  }
}); 
</code></pre>

<hr />
<h3>Retrieving User Properties</h3>
<p>This function returns an object with your user properties.</p>
<p><strong>Note</strong>: This function should be called after a successful Login.</p>
<pre><code>relayr.user().getUserInfo();
</code></pre>

<hr />
<h2>Example: Receiving Data from your Sensors without logging in</h2>
<p>We've made it possible for you to start viewing your sensors' data without having to provide an actual <code>redirectUri</code> and without having to log in to the platform.</p>
<p>In this scenario you will need to perform the initialization of the SDK, with the <strong>AppID</strong> of your app.</p>
<pre><code>  var relayr = RELAYR.init({
    appId: &quot;{YourAppId}&quot;
  });
</code></pre>

<p>And provide your <strong>DeviceID</strong> and <strong>Token</strong> in order to start receiving data. See instructions for generating your token <a href="https://developer.relayr.io/documents/Browser/OAuthToken" target="_blank">here</a>. Your Device ID can be obtained from the <a href="https://developer.relayr.io/dashboard/devices" target="_blank">Devices section</a>, by clicking on the 'settings' icon on any of the devices. </p>
<pre><code>  relayr.devices().getDeviceData({
    deviceId: &quot;{YourDeviceId}&quot;, 
    token: token,
    incomingData: function(data){
      console.log(&quot;sensor&quot;,data);
    }
  });    
</code></pre>

<hr />
<p>Now that you understand how to incorporate receiving data via the relayr platform. You can go ahead and create your first web project. Check out our simple and quick <a href="https://github.com/relayr/cantTouchThis" target="_blank">Can't Touch This sample project</a> to get inspired.</p>