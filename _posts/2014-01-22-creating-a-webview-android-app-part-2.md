---
layout: post
title: Creating a Basic Android App - Part 2
subtitle: Interacting with Javascript.
excerpt: Using native features, through Javascript.
categories: tutorial android
collection: tutorial
comments: true
published: true
---

This follows on from the [previous post](http://blog.steveedson.co.uk/tutorial/android/2014/01/21/creating-a-webview-android-app.html).

### JavaScriptInterface

At this stage, we have a working app that can load a webpage, with Javascript enabled. The next step is to allow us to use the native features on the device, through the webview. Examples of this include:

- Geolocation
- Device information, such as battery levels, network status.
- Controlling hardware, such as the camera and vibration.
- Native sharing, using the apps installed on the device.
- Anything else that the device is capable of.

A new class needs to be created in the `MainActivity.java` file, after the `onCreate()` method, like so:

```java
public class jsBridge {
    Context mContext;

    /** Instantiate the interface and set the context */
    jsBridge(Context c) {
        mContext = c;
    }

    /** Show a toast from the web page */
    @JavascriptInterface
    public void showMessage(String message) {
        Toast.makeText(mContext, message, Toast.LENGTH_SHORT).show();
    }
}
```
*Note:* If eclipse shows a load of errors for some of the variables, hover over the error, and select `import`___.

We then assign this to our `webView`, like so

```java
webView.addJavascriptInterface(new jsBridge(this), "Android");
```

Then, on our HTML page, we can add the Javascript that would natively show a message on the device:

```html
<script type="text/javascript">
    Android.showMessage("Hello!");
</script>
```

There are hundreds more features that can be added to the `JavaScriptInterface`, for example, controlling the camera, recording audio, making the device vibrate, enabling WiFi, using the compass or getting information like the battery level.

For more examples of specific features, leave a comment.
