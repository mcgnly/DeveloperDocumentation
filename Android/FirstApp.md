# A first Android App

## Preliminaries
#### Download [Android Studio](https://developer.android.com/sdk/installing/studio.html)

#### Open Android Studio
#### Start a new Project
    Give you application a name - I've chosen "Thermometer"

    The company name is actually what creates the central Java package. In the
    java world packages are where you keep your source file which will get
    compiled, they are a good way of organising your code, and not letting it
    conflict with someone elses code.

    The best thing is to use your webadress so for me that's relayr.io. I'm
    going to put demo at the front of the name too so demo.relayr.io. Studio
    uses this name but backwards (!!! it's a convention thing) to create a
    package.

#### Select form factors

    I'm interested in creating an app that will run on phones and tablets so
    I've selected that option. Minimum SDK defines on which phones the app will
    run. Android 4.0.3 is the oldest version with support for BlueTooth so I've
    chosen that one. I could choose a newer SDK but then older phones would not
    be able to run the app. 

#### Add activity to mobile Phone

    Activitys are androids way of describing screens that a user can interact
    with - we'll be needing one of those.  I've just chosen Blank Activity here.

#### Choose Options for the new Activity

    I'm going to call my screen ActivityThermometerDemo. Android Studio is
    smart enough to fill out the other fields for me.

#### Finish!

    Once you click 'finish' Android Studio goes off and builds your basic
    project for you. It can take a little while for it to create all the files
    so be patient.

## Enablng the relayr SDK

    To make it easy to develop an App to use the WunderBar we've developed a
    Software Development Kit (SDK), containing a lot of code which you can use.
    You will still need to write your own app, but our SDK makes some of the
    harder parts simple.

    Anddroid Studio doesn't include our SDK so we'll have to show it where you
    can find it. The Studio uses another tool call gradle to compile and run
    code. Gradle in turn uses another tool called Maven to find missing bits of
    code (there are a lot of tools involved in programming). We've uploaded our
    SDK to Maven Central, so that it can be downloaded by anyone.

    In the left panel of the Studio you can see the file structure of our
    Thermometer project (if you can't see the Project, you can click on the
            small 1: Project tab on the left hand side or use Command+1 to open
            it)
    Open the app tab and inside you should find a file called build.gradle -
    open it!

    ### Editing the build file

    In the file you will see some **code blocks** these are single-word names
    followed by a pair of curly braces (or brackets) and inside there are
    properties.

    At the bottom of the file you will see a 'dependencies' block, with a
    'compile' instruction. We're going to replace the 'filetree' with a
    reference to the Maven repository where the relayr SDK is stored.

        dependencies {
	        compile 'io.relayr:android-sdk:0.0.1'
	    }

    Once I've saved the file, I can check to see if I have everything correct
    so I choose to 'Rebuild' the project from the 'build' menu. If I made a
    mistake I'll get a big error warning. This time I (deliberately) mispelled
    the relayr name. I fix the mistake and rebuild and the Error goes away.

    Now we are ready for the next step!

## Programme your Application

    ### create the Application class.
    Back in the Project window you should open app/src/main/java. You should
    see another folder named after the package you defined when creating your
    project plus the name of your app - io.relayr.demo.thermometer. Inside you
    will see the Activity that Studio made for you when you created your new
    app. Select the Package and then 'new' from the 'file' menu, or right click
    on the package and select 'new', we want to craete a Java Class, so choose
    that option. 

    you will be asked to give your new class a Name. I chose
    'ThermometerDemoApplication'. Studio creates a calss for me which has the
    right name. However I want my Application to be a type of
    android.app.Application. Otherwise Android won't be able to recognise it.
    To do this we **extend** the android Application class. extending means
    that it has everything from the Application class, plus whatever I add into
    the class.

    Start Typing 'extends' after the class name, Studio will suggest
    'autcompleting' this for you, which makes it fatser - give it a go! now
    start typing 'Application' and again it will suggest
    app.android.Application. What is geat here, is that it doesn't just
    complete the word, but also adds the **import** statement at the top, which
    tells java which 'Application' you mean.

    ### Add an onCreate Handler
    As different things happen in an app messages called 'Events' are sent
    around to any classes which might be interested. we add 'Event handlers' to
    our code if we want to do something interesting when one of those events
    occur. In our app we are interested in our app being launched. When this
    happens a 'create' event is sent, so we create an onCreate handler. You can
    choose events already defined the Application class by choosing 'Override
    methods' from the Code menu, or just copy and paste like this:

        @Override
        public void onCreate() {
            super.onCreate();
        }
    
    This method doesn't do anything yet. We are going to add a single line
    which sets up the relayr SDK so we can use it.

        @Override
        public void onCreate() {
            super.onCreate();
            RelayrSdk.initSdk(this);
        }

    At the moment this class doesn't know about the RelayrSdk so it marks it
    red as an error. We need to add another import statement so that Studio can
    find the word. The easiest way is to select the word, Studio suggests the
    import stament which you can select by typing opt+Enter  

    So it's not exciting yet, but we have an Application Class. Now let's give
    our app some capabilites

## Letting our App connect to the internet

    We need to tell Anroid that our app wants to connect to the internet. to do
    that we should edit the manifest file. you can find it again in the project
    view: app/src/main/AndroidManifest.xml. The Manifest tells Android a lot of
    things about your app so lets tell it that we want to connect to the
    Internet. In the space between the <manifest blahblah> tag and the
    <application blahblah> tag we'll add the following:

        <uses-permission android:name="android.permission.INTERNET" />

    If you put it in the wrong place you'll have a wobbly red line to tel you
    about the mistake so pay attention!.

    We also want to tell Andorid which class is our main application class. so
    we neet to add it to the <application> tag. right after
    'android:theme="@style/AppTheme"' you can add the following:

    android:name=".ApplicationThermometerDemo"

    if you type, then Studios autocomplete will help you fill in the right
    details. 

    Now we've told Android what it needs to know about our app we're done here.

## Letting Users to Login

    SO that your app can connect to your Connected devices (or those of the
            user) you'll need to be able to login. Whilst we have wrapped a lot
    of complications away in the SDK, you'll still have to create the views
    that use that logic. This next section has a lot more code.

    ### Adding some strings.
    Strings are just letters, words and numbers, usually things that you want
    to read. Because you wnat to be able to easily change text, Android puts
    texts in a special file, which is easy to change - and to translate into
    other languages. In the Project panel open up app/src/main/res/values/strings.xml

    right after the <string> tags that are already add the following:

        <string name="action_log_in">Log in</string>
        <string name="action_log_out">Log out</string>
        <string name="successfully_logged_in">You were successfully logged in!</string>
        <string name="successfully_logged_out">You were successfully logged out!</string>
        <string name="unsuccessfully_logged_in">There was a problem in the log in.</string>

    We'll use these in a moment when we define labels on buttons, and messages
    for our user.

    ### Add a log-in view

    right click (or ctrl-click) on the app/src/res/menu folder and choose new >
    "Menu Resource file" and give it the name
    "thermometer_demo_not_logged_in.xml" it will create a file with the code
    like this:

        <?xml version="1.0" encoding="utf-8"?>

        <menu xmlns:android="http://schemas.android.com/apk/res/android">

        </menu>

    We need to add some information to tel Andorid that this menu belongs with
    our thermometer Activity:

        <?xml version="1.0" encoding="utf-8"?>

        <menu xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            tools:context=".ThermometerDemoActivity" >

        </menu>

    The first new attribute loads the android tools, and in the second we tell
    it to run in "the Context" of the Activity.

    now we can add the content to the menu.


        <menu xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            tools:context=".ThermometerDemoActivity" >
            <item android:id="@+id/action_log_in"
                  android:title="@string/action_log_in"
                  android:orderInCategory="100"
                  android:showAsAction="never" />
        </menu>

    Our menu has only a single item, the title (an text) of that item is the
    one we defined earlier in the strings.xml file.

    ### adding a logout menu
    This is the evil twin brother of login. We'll call him thermometer_demo_logged_in.xm, and copy the
    following content there:

        <menu xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            tools:context=".ThermometerDemoActivity" >
            <item android:id="@+id/action_log_out"
                  android:title="@string/action_log_out"
                  android:orderInCategory="100"
                  android:showAsAction="never" />
        </menu>

     If you have the preivew pane active, you should now notice that a little
     "log in" menu item appears on the screen.

    ### Adding the logic to the Activity
    
    If you remember earlier I mentioned that an Activity is a an interactive
    screen within an App. WE have quite a lot to do here, so I'm going to put
    the whole code in and add code comments:

        package io.relayr.demo.thermometer;

        //import the Android classes we will need
        import android.app.Activity;
        import android.os.Bundle;
        import android.view.Menu;
        import android.view.MenuItem;
        import android.widget.Toast;


        //import the relayr LoginEventListener
        import io.relayr.LoginEventListener;

        //and the relayr SDK
        import io.relayr.RelayrSdk;


        public class ActivityThermometerDemo extends Activity implements LoginEventListener {


            /**
             * Once the Activity has been started the onCreate method will be called
             * @param savedInstanceState
             */
            @Override
            protected void onCreate(Bundle savedInstanceState) {
                super.onCreate(savedInstanceState);

                //we load the layout xml defined in app/src/main/res/layout
                setContentView(R.layout.activity_thermometer_demo);

                //we use the relayr SDK to see if the user is logged in by
                //caling the isUserLoggedIn function
                if (!RelayrSdk.isUserLoggedIn()) {

                    //if the user isn't logged in (the little exclamation mark
                    //is the same as saying not) then we call the logIn method
                    RelayrSdk.logIn(this, this);
                }
            }

            /**
             * When Android is ready to draw any menus it fires the
             * "prepareOptionsMenu" Event, this method is caled to handle that
             * event.
             * @param menu
             * @return
             */
            @Override
            public boolean onPrepareOptionsMenu(Menu menu) {

                //remove any previous stuff in the menu
                menu.clear();

                //Check to see if the user is logged in
                if (RelayrSdk.isUserLoggedIn()) {

                    //if the user is logged in we ask Android to draw the menu
                    //we defined earlier in the thermometer_demo_logged_in.xml
                    //file
                    getMenuInflater().inflate(R.menu.thermometer_demo_logged_in, menu);
                } else {

                    //otherwise we return the
                    //thermometer_demo_not_logged_in.xml file
                    getMenuInflater().inflate(R.menu.thermometer_demo_not_logged_in, menu);
                }

                //we must return this, so that any other classes interested in
                //the prepare menu event can do something.
                return super.onPrepareOptionsMenu(menu);
            }

            /**
              * If a menu item is selected, we see which item was called and
              * decide what to do.
              */
            @Override
            public boolean onOptionsItemSelected(MenuItem item) {

                //if the user slected login
                if (item.getItemId() == R.id.action_log_in) {

                    //we call the login method on the relayr SDK
                    RelayrSdk.logIn(this, this);
                    return true;
                } else if (item.getItemId() == R.id.action_log_out) {

                    //otherwise we call the logout method defined later in this
                    //class
                    logOut();
                    return true;
                }
                return super.onOptionsItemSelected(item);
            }

            /**
             * Called when the user logs out
             */
            private void logOut() {

                //call the logOut method on the reayr SDK
                RelayrSdk.logOut();

                //call the invalidateOptionsMenu this is defined in the
                //Activity class and is used to reset the menu option
                invalidateOptionsMenu();

                //use the Toast library to display a message to the user
                Toast.makeText(this, R.string.successfully_logged_out, Toast.LENGTH_SHORT).show();
            }

            /**
              * When a user successfuly logs in, the SuccessUserLogin event is
              * fired, and we handle that here.
              */
            @Override
            public void onSuccessUserLogIn() {

                //use the Toast library to display a message to the user
                Toast.makeText(this, R.string.successfully_logged_in, Toast.LENGTH_SHORT).show();
                //call the invalidateOptionsMenu this is defined in the
                //Activity class and is used to reset the menu option
                invalidateOptionsMenu();
            }

            /**
              * if their is a problem logging in the user, the ErrorLogin evet
              * is fired and handled here.
              */
            @Override
            public void onErrorLogin(Throwable e) {
                //use the Toast library to display a message to the user
                Toast.makeText(this, R.string.unsuccessfully_logged_in, Toast.LENGTH_SHORT).show();
            }
        }

    That was a lot of code in one lump. I hope you culd follow some of what was
    going on :) WE need to do one last thing now, which is to craete a new app
    in the dashboard

    ####  Create a relayr app on the [app page](https://developer.relayr.io/dashboard/apps/myApps). 
    Download the key file once the app is created. This Key file, is used by
    the SDK to tell the relayr Platform which app you are, and to make sure
    that you are allowed to ruun there - it's like a username and password,
    just for your app.

    You will need to creat a new directory called app/src/main/assets and copy
    the file into that folder.


## Greeting the user
    Once the user has logged in - it would be nice to say Hi! WE're going to
    need some more code in the activity screen for that


        package io.relayr.demo.thermometer;

        //import the Android classes we will need
        import android.app.Activity;
        import android.os.Bundle;
        import android.view.Menu;
        import android.view.MenuItem;
        import android.widget.Toast;

        //add a couple more views
        import android.view.View;
        import android.widget.TextView;


        //import the relayr LoginEventListener
        import io.relayr.LoginEventListener;

        //and the relayr SDK
        import io.relayr.RelayrSdk;

        //import the relay User
        import io.relayr.model.User;

        //import more android classes
        import rx.Subscriber;
        import rx.Subscription;
        import rx.android.schedulers.AndroidSchedulers;
        import rx.schedulers.Schedulers;

        public class ActivityThermometerDemo extends Activity implements LoginEventListener {


            /**
             * Once the Activity has been started the onCreate method will be called
             * @param savedInstanceState
             */
            @Override
            protected void onCreate(Bundle savedInstanceState) {
                super.onCreate(savedInstanceState);

                //we load the layout xml defined in app/src/main/res/layout
                setContentView(R.layout.activity_thermometer_demo);

                //we use the relayr SDK to see if the user is logged in by
                //caling the isUserLoggedIn function
                if (!RelayrSdk.isUserLoggedIn()) {

                    //if the user isn't logged in (the little exclamation mark
                    //is the same as saying not) then we call the logIn method
                    RelayrSdk.logIn(this, this);
                }
            }

            /**
             * When Android is ready to draw any menus it fires the
             * "prepareOptionsMenu" Event, this method is caled to handle that
             * event.
             * @param menu
             * @return
             */
            @Override
            public boolean onPrepareOptionsMenu(Menu menu) {

                //remove any previous stuff in the menu
                menu.clear();

                //Check to see if the user is logged in
                if (RelayrSdk.isUserLoggedIn()) {

                    //if the user is logged in we ask Android to draw the menu
                    //we defined earlier in the thermometer_demo_logged_in.xml
                    //file
                    getMenuInflater().inflate(R.menu.thermometer_demo_logged_in, menu);
                } else {

                    //otherwise we return the
                    //thermometer_demo_not_logged_in.xml file
                    getMenuInflater().inflate(R.menu.thermometer_demo_not_logged_in, menu);
                }

                //we must return this, so that any other classes interested in
                //the prepare menu event can do something.
                return super.onPrepareOptionsMenu(menu);
            }

            /**
              * If a menu item is selected, we see which item was called and
              * decide what to do.
              */
            @Override
            public boolean onOptionsItemSelected(MenuItem item) {

                //if the user slected login
                if (item.getItemId() == R.id.action_log_in) {

                    //we call the login method on the relayr SDK
                    RelayrSdk.logIn(this, this);
                    return true;
                } else if (item.getItemId() == R.id.action_log_out) {

                    //otherwise we call the logout method defined later in this
                    //class
                    logOut();
                    return true;
                }
                return super.onOptionsItemSelected(item);
            }

            /**
             * Called when the user logs out
             */
            private void logOut() {

                //call the logOut method on the reayr SDK
                RelayrSdk.logOut();

                //call the invalidateOptionsMenu this is defined in the
                //Activity class and is used to reset the menu option
                invalidateOptionsMenu();

                //use the Toast library to display a message to the user
                Toast.makeText(this, R.string.successfully_logged_out, Toast.LENGTH_SHORT).show();
            }

            /**
              * When a user successfuly logs in, the SuccessUserLogin event is
              * fired, and we handle that here.
              */
            @Override
            public void onSuccessUserLogIn() {

                //use the Toast library to display a message to the user
                Toast.makeText(this, R.string.successfully_logged_in, Toast.LENGTH_SHORT).show();
                //call the invalidateOptionsMenu this is defined in the
                //Activity class and is used to reset the menu option
                invalidateOptionsMenu();
            }

            /**
              * if their is a problem logging in the user, the ErrorLogin evet
              * is fired and handled here.
              */
            @Override
            public void onErrorLogin(Throwable e) {
                //use the Toast library to display a message to the user
                Toast.makeText(this, R.string.unsuccessfully_logged_in, Toast.LENGTH_SHORT).show();
            }
        }


     



## Running your App in a virtual phone

    ### Install a Virtual phone
    Android has a range of Virtual Phone (and tablets and tvs, etc) which are
    managed by something called the Virtual Device Manager (VDM), but this
    doesn't have any Virtual devices either. You will need to download them
    first. Luckily there is yet another tool called the SDK MAnager that you
    can use to download Virtual systems.

    To open the SDK Manager you'll need to find the tiny icon of a robot
    sticking his head above a download arrow. clcik this and the SDK amanger
    will open

    My brand new installation found 8 updates, so I installed them first. You
    have to slect each package and agree to the license. This took a while to
    download but gave me some Virtual Hardware. So I opened the AVD - this is
    the icon next to SDK MAnager.

    I hit he create button to create a new device. In the wndow that came up I
    gave it a name and selcted Nexus5. you have to tell it which CPU, ARM is
    the Option closest to the real phone, but the INtel option is closer to the
    computer you are running on so is probably faster. I chose a skin with
    HArdware Dynamic controls, and left everything else as I found it.

    Now from within Studio you can run your app. choose "Run 'app'" from the
    "Run" menu, select launch emulator and choose the virtual device you just
    defined.


