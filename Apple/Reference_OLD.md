# The relayr iOS Framework 

The relayr iOS SDK provides iOS app developers with a number of programmatic access points into the relayr API. Simpy follow the steps below to install the framework and integrate it into your project. For the complete list of endpoints please see  <a href="https://developer.relayr.io/documents/iOS/Endpoints" target="_blank"> https://developer.relayr.io/documents/iOS/Endpoints </a>

## Setup

* Download the Relayr.framework from <a href="https://github.com/relayr/ios-sdk/releases" target="_blank"> https://github.com/relayr/ios-sdk/releases </a> 
* Create a new project in XCode
* Drag and drop the following frameworks/libraries into XCode:
	
	* Relayr.framework
	* libz.dylib
	* SystemConfiguration.framework
	* CFNetwork.framework
	* CoreBluetooth.framework
	* CoreGraphics.framework
	* UIKit.framework
	* Foundation.framework

![](assets/framework.png)

In addition, place an import statemtent in the Prefix.pch file

**This is an example of a Prefix.pch containing the statement:**

	
			
			#import <Availability.h>
		
			#ifndef __IPHONE_5_0
			#warning "This project uses features only available in iOS SDK 5.0 and later."
			#endif
			
			#ifdef __OBJC__
			  #import <UIKit/UIKit.h>
			  #import <Foundation/Foundation.h>
			  #import <Relayr/Relayr.h>
			#endif
	

