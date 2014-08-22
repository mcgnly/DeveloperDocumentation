# Creating a Login Sequence

In order to enable your app to connect to your onboarded devices (or those of the
user) you'll need to be able to login. 
Though we have wrapped a lot of complications away in the SDK, you'll still have to create the views that use that logic. 

## Adding String Names

In the Project panel open up app/src/main/res/values/strings.xml

After the existing `<string>` tags add the following:

    <string name="action_log_in">Log in</string>
    <string name="action_log_out">Log out</string>
    <string name="successfully_logged_in">You were successfully logged in!</string>
    <string name="successfully_logged_out">You were successfully logged out!</string>
    <string name="unsuccessfully_logged_in">There was a problem in the log in.</string>

these will be used to define labels on buttons and messages
for user interaction. 

## Adding a Log-in View

Right click (or ctrl-click) the app/src/res/menu folder and choose new >
"Menu Resource file" and give it a name.

In the example below we have used: *"thermometer_demo_not_logged_in.xml"*.
It will create a file with the following code:


    <?xml version="1.0" encoding="utf-8"?>

    <menu xmlns:android="http://schemas.android.com/apk/res/android">

    </menu>

Add the following to link between the view and the activity you've defined when creating the project. In our example below, the Activity that was defined was `ThermometerDemoActivity`

    <?xml version="1.0" encoding="utf-8"?>

    <menu xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        tools:context=".ThermometerDemoActivity" >

    </menu>

The first new attribute loads android tools, and in the second we indicate that it should run in "the Context" of the Activity we had defined.


Now we can add the content to the menu.


    <menu xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        tools:context=".ThermometerDemoActivity" >
        <item android:id="@+id/action_log_in"
              android:title="@string/action_log_in"
              android:orderInCategory="100"
              android:showAsAction="never" />
    </menu>

Our menu has only a single item, the title of that item is the
one we defined earlier in the *strings.xml* file.

## Adding a Logout Menu

This is the evil twin brother of login. We'll call it *thermometer_demo_logged_in.xml*, and copy the following content into it:

    <menu xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        tools:context=".ThermometerDemoActivity" >
        <item android:id="@+id/action_log_out"
              android:title="@string/action_log_out"
              android:orderInCategory="100"
              android:showAsAction="never" />
    </menu>

If you have the preview pane active, you should now notice a little
"Log in" menu item appear on the screen.

## Adding Logic to the Activity

We've mentioned that an Activity is a an interactive screen within an App. 
The following code is used to build the logic behind the activity. It's a bit long, but we've added code comments to walk you through it... See you on the other side

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
         * When Android is ready to draw any menus it initiates the
         * "prepareOptionsMenu" event, this method is caled to handle that
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
          * When a menu item is selected, we see which item was called and
          * decide what to do upon it.
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
          * fired, and we handle it here:
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
          * if there is a problem logging in a user, the ErrorLogin evet
          * is initiated and handled here.
          */
        @Override
        public void onErrorLogin(Throwable e) {
            //use the Toast library to display a message to the user
            Toast.makeText(this, R.string.unsuccessfully_logged_in, Toast.LENGTH_SHORT).show();
        }
    }


So, now that you are en expert in building Login sequences, get back to the <a href="https://developer.relayr.io/documents/Android/GettingStarted"> Getting Started </a> menu to continue building your app.