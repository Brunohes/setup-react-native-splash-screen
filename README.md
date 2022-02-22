# setup-react-native-splash-screen

A document to remember how to properly setup a splash screen in React Native both IOS and Android.

You can use this site to generate all SplashLogo Sizes - https://appicon.co/#image-sets

## General

- yarn add react-native-splash-screen
- cd ios
- pod install

- Add this to file on top of aplication: (Ex: app.tsx)

```
import SplashScreen from 'react-native-splash-screen'

useEffect(() => {
  SplashScreen.hide()
})
```

## Android

- Put the splash_logo.png's in the path android/app/src/main/res in the mipmap-\* folder accordly with their sizes.
- In android/app/src/main/res/drawable let's create a file background_splash.xml

```
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:drawable="@color/splashscreen_bg"/>

    <item
        android:width="300dp"
        android:height="300dp"
        android:drawable="@mipmap/splash_logo"
        android:gravity="center" />

</layer-list>
```

- In android/app/src/main/res/values if not created yet lets create colors.xml

```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="splashscreen_bg">#000</color>
    <color name="app_bg">#000</color>
</resources>
```

- Update android/app/src/main/res/values/styles.xml to add our theme.

```

<resources>

    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
        <!-- Customize your theme here. -->
        <item name="android:textColor">#000000</item>

        <!-- Add the following line to set the default status bar background color for all the app. -->
        <item name="android:statusBarColor">@color/app_bg</item>
        <!-- Add the following line to set the default status bar text color for all the app
        to be a light color (false) or a dark color (true) -->
        <item name="android:windowLightStatusBar">false</item>
        <!-- Add the following line to set the default background color for all the app. -->
        <item name="android:windowBackground">@color/app_bg</item>
    </style>

    <!-- Adds the splash screen definition -->
    <style name="SplashTheme" parent="Theme.AppCompat.Light.NoActionBar">
        <item name="android:statusBarColor">@color/splashscreen_bg</item>
        <item name="android:background">@drawable/background_splash</item>
    </style>

</resources>
```

- Update AndroidManifest.xml

```

<manifest xmlns:android="http://schemas.android.com/apk/res/android"
  package="com.wutangapp">

    <uses-permission android:name="android.permission.INTERNET" />

    <application
      android:name=".MainApplication"
      android:label="@string/app_name"
      android:icon="@mipmap/ic_launcher"
      android:roundIcon="@mipmap/ic_launcher_round"
      android:allowBackup="false"
      android:theme="@style/AppTheme">

      <!-- Add SplashActivity -->
        <activity
          android:name=".SplashActivity"
          android:theme="@style/SplashTheme"
          android:label="@string/app_name"
		  android:exported="false"
		  >
          <intent-filter>
              <action android:name="android.intent.action.MAIN" />
              <category android:name="android.intent.category.LAUNCHER" />
          </intent-filter>
        </activity>

      <!-- Remove intent-filter on MainActivity and add the parameter android:exported="true" -->
      <activity
        android:name=".MainActivity"
        android:label="@string/app_name"
        android:configChanges="keyboard|keyboardHidden|orientation|screenSize|uiMode"
        android:launchMode="singleTask"
        android:windowSoftInputMode="adjustResize"
        android:exported="true"/>
      <activity android:name="com.facebook.react.devsupport.DevSettingsActivity" />
    </application>

</manifest>
```

- Create the File android/app/src/main/java/[package_name]/SplashActivity.java

```
package com.packagename; // This should be your package name!!!.

import android.content.Intent;
import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;

public class SplashActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        Intent intent = new Intent(this, MainActivity.class);
        startActivity(intent);
        finish();
    }
}
```

- Edit the file android/app/src/main/java/[package_name]/MainActivity.java

```
package com.packagename; // This should be your package name.

import com.facebook.react.ReactActivity;
import org.devio.rn.splashscreen.SplashScreen; // Import this.
import android.os.Bundle; // Import this.

public class MainActivity extends ReactActivity {
    // Add this method.
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        SplashScreen.show(this);
        super.onCreate(savedInstanceState);
    }

    ...
}
```

- Let's create the file android/app/src/main/res/layout/launch_screen.xml

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@drawable/background_splash"
    android:orientation="vertical">
</LinearLayout>
```

Done!

## IOS

- Open XCode:
  - Create a new image in folder Images.xcassets with the name SplashLogo.
  - Drag and drop images according to their sizes.
- Edit the file LaunchScreen.Storyboard:
  - Remove unused.
  - Add SplashLogo and constrains.
  - Change Background color.
- On the file AppDelegate.m:

  - On top add `#import "RNSplashScreen.h"`
  - In the method didFinishLaunchingWithOptions add `[RNSplashScreen show];` above `return YES;`.

- On the file info.plist:
  - Add a row `Status bar style` with the option `Light Content` for darker background and `Dark Content` for lighter background.

Done!
