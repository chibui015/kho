Here is how to scan a barcode from another Android application via `Intent`.

## IntentIntegrator

The best way to integrate is to use the small library of code provided. It correctly handles for you many details, such as setting category, flags, picking the most appropriate app, and most importantly handling the case where Barcode Scanner is not installed:

* https://github.com/zxing/zxing/blob/master/android-integration/src/main/java/com/google/zxing/integration/android/IntentIntegrator.java
* https://github.com/zxing/zxing/blob/master/android-integration/src/main/java/com/google/zxing/integration/android/IntentResult.java

See the javadoc of the class to see how to use it. First add code to invoke the Intent:

```
IntentIntegrator integrator = new IntentIntegrator(yourActivity);
integrator.initiateScan();
```

Second, add this to your `Activity` to handle the result:

```
public void onActivityResult(int requestCode, int resultCode, Intent intent) {
  IntentResult scanResult = IntentIntegrator.parseActivityResult(requestCode, resultCode, intent);
  if (scanResult != null) {
    // handle scan result
  }
  // else continue with any other code you need in the method
  ...
}
```

There are more methods an options in this class which allow you to customize the request, or perform other actions like sharing text via QR code.

## Via URL

You can also launch the app from a URL in the Browser. Create a hyperlink to http://zxing.appspot.com/scan and Barcode Scanner will offer to launch to handle it. Users can also choose to always have Barcode Scanner open automatically.

This URL is not meant to serve an actual web page in a browser; it's just a hook to launch a native app.

## Manually

You can reuse code from `IntentIntegrator.java` if you'd like to customize it significantly.

For more options, like scanning a product barcode, or asking Barcode Scanner to encode and display a barcode for you, see this source file:

https://github.com/zxing/zxing/blob/master/android/src/com/google/zxing/client/android/Intents.java

And here's some source from the test app which shows how to use them:

https://github.com/zxing/zxing/blob/master/androidtest/src/com/google/zxing/client/androidtest/ZXingTestActivity.java