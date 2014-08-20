# Setting up your Environment

Here are a few preliminary steps you would need to follow in order to get your development environment all ready for building you first app. 

#### Download [Android Studio](https://developer.android.com/sdk/installing/studio.html)

----------

#### Create an Android Project 

The minimum supported version for the Android project is 15 (4.0.3).

See the Android [Thermometer demo](https://github.com/relayr/android-demo-apps/commit/3e33f01c7e693e5ee0f9884dea8218731b8c9c8b) for detailed instructions. 

----------


#### Create an empty Git repository or re-initialize an existing one:
	    
	    git init

----------

	    
#### Download the [relayr Android SDK](https://developer.relayr.io/dashboard/sdk) 

----------


####  Add the android relayr SDK as a git sub-module: 
  
	    git submodule add https://github.com/relayr/android-sdk.git libraries/relayr-sdk



----------

	   
####  Reference the relayr SDK project in your settings.gradle file: 
	    
	    include ':app', ':libraries:relayr-sdk:android-sdk'


----------

	    
####  Reference the relayr SDK project in the build.gradle file inside your app folder:
	    
	    dependencies {
	        compile project(':libraries:relayr-sdk:android-sdk')
	    }

See example [here](https://github.com/relayr/android-demo-apps/commit/f2c17c6a9a20f0c0e1f12cf8c38c2afd5ed4449d) 

----------

#### Now that you are done setting up your environment, click [here](/GettingStarted) to go back to the Getting Started page.