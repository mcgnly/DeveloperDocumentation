# Setting up your Environment

Here are a few preliminary steps you would need to follow in order to get your development environment all ready for building you first app. 

### Download [Android Studio](https://developer.android.com/sdk/installing/studio.html)

----------

### Create an Android Project on Android Studio 

#### Give your project a name
<img src="assets/Project.png" class="center">

#### Set the minimum supported version
The minimum supported version for the relayr Android project is 15 (4.0.3).

<img src="assets/Version.png" class="center">

#### Select an Android activity

<img src="assets/Activity.png" class="center">

#### Choose your activity options

<img src="assets/ActivityOpt.png" class="center">

#### Click "Finish" to start the building process of your initial project

<img src="assets/Building.png" class="center"> 


----------

   
###  Enabling the relayr Android SDK 

We have made the relayr Android SDK available on Maven as to simplify the inclusion process.

#### Open the *build.gradle* file inside your app folder (*app/build.gradle*)

<img src="assets/SDKInclusion.png" class="center">

#### Add the following to your dependencies:
	    
	    dependencies {
	        compile 'io.relayr:android-sdk:0.0.5'
	    }

#### Click to save the file or compile to ensure that everything is in order.

----------

#### Now that you are done setting up your environment, click [here](https://developer.relayr.io/documents/Android/GettingStarted) to go back to the Getting Started page.