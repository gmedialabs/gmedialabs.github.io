---
layout: post
title: Creating a Basic Android App - Part 1
subtitle: Introduction and WebView. 
excerpt: How to create a basic Android app running a WebView.
categories: tutorial android
comments: true
---

### Setting up the SDK

- Download and install the Android SDK from the official website.
    - Either [Android Studio](http://developer.android.com/sdk/installing/studio.html) or [Eclipse (ADT)](http://developer.android.com/sdk/index.html) can be used. Eclipse (ADT) will be used in this example.

### Create virtual machine

Once downloaded and installed, an Android virtual device can be created. [Intructions can be found here.](http://developer.android.com/tools/devices/managing-avds.html)

### Create our application

Next, we can start to create our project, like so.

![Create project](/images/1-new-project.png)

![Add details](/images/2-new-app-details.png)

![Configure](/images/3-configure.png)

![Create fancy icon](/images/4-icon.png)

![Create blank activity](/images/5-create-activity.png)

The app will have been created, close the android welcome tab to show the editor.

### Directory structure

The following directory structure should have been created:

![Directory Structure](/images/7-directories.png)

In this example, the main folders that will be used are `src` and `res`.

- The `src` folder contains all the java code that powers the app.
- The `res` folder contains assets, such as images (known as drawables), and layouts.
	- The `drawable-hdpi` etc folders are used to target different screen sizes, this is like using media queries.


### Add internet permission

To be able to connect to the internet, the internet permission is needed. In `AndroidManifest.xml`, add the following line:

```xml
<uses-permission android:name="android.permission.INTERNET"></uses-permission>
```

The app will now be allowed to use the internet to retrieve data etc.

### Add webview layout

In Android apps, layouts are defined in XML files. Think of these as being like HTML + CSS files, they define which views should be used and how they should appear.

Open the file, `res/layout/activity_main.xml`. This is the layout for our main activity, created earlier.

The webview can be added either using the GUI editor, or in XML. We will use XML in this example.

```xml

<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity" >

    <WebView
        android:id="@+id/webView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</RelativeLayout>

```
Edit the file so it looks like the one above.

- The relative layout is the root view, think of this as being like the `<body>` of a HTML page.
-  `layout_width` and `layout_height` can take multiple values, `match_parent` means match the same size as the parent element, in this case, the same as the actual screensize.
-  Think of `xmlns:tools="http://schemas.android.com/tools"` as being similar to a HTML Doctype, it just explains how the xml should be parsed.
-  The ID of the WebView is `webView`, this is similar to setting an ID on a HTML element, it allows the view to be targetted later on.

### Using the WebView

Now that the WebView has been added to the layout, we can interact with it. Open the `MainActivity.java` file within the `src` folder. It should look like this:

```java
package com.steveedson.coursework;

import android.os.Bundle;
import android.app.Activity;
import android.view.Menu;

public class MainActivity extends Activity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.main, menu);
        return true;
    }
}

```

The `onCreate` method is called when the activity is first started, it is part of the [Android Lifecycle](http://developer.android.com/training/basics/activity-lifecycle/starting.html).

Create the variable, `webView`, this will be assigned to the WebView object later.

![Import Webview](/images/9-import-webview.png)

Inside the `onCreate` method, add the line

```java
webView = (WebView) findViewById(R.id.webView);
```

- `findViewById` is used to retrieved a view from our layout.
- `R.id.{whatever}`: The `R` object stores all the resources available in the app.
- Finally, the `WebView` part `casts` the layout as a webview. This is because the `findViewById` method could return many different types of views, including `TextView`, `ListView` etc.

Next, set up the `WebChromeClient` and `WebViewClient`, these are used to control the UI, and the webview itself, respectively. These will be used later.

```java
webView.setWebChromeClient(new WebChromeClient() {
      public void onProgressChanged(WebView view, int progress) {
		// Page is loading
	}
});

webView.setWebViewClient(new WebViewClient() {        
      @Override
      public void onReceivedError(WebView view, int errorCode, String description, String url) {
           // Received error when loading page.
      }
});
```

We can display a loading bar in the app by enabling it in the `onCreate` method:

```java
final Activity activity = this;
getWindow().requestFeature(Window.FEATURE_PROGRESS);
```
and updating it, as the page loads, inside the `onProgressChanged()` method:

```java
activity.setProgress(progress * 1000);
```

#### Enabling Javascript

While [not normally recommended](http://developer.android.com/reference/android/webkit/WebView.html#addJavascriptInterface(java.lang.Object, java.lang.String), we need to enable JavaScript on our webview, this can be done like so:

```java
WebSettings settings = webView.getSettings();
settings.setJavaScriptEnabled(true);
```


Finally, tell the webview to load a URL, like so:

```java
webView.loadUrl("http://google.com");
```

Test the app in the emulator to make sure everything is ok.

![Everything looks ok](/images/10-app-test-1.png)
