## Android / Barcode Scanner

### Why does Barcode Scanner request permission to access ...

#### ... my contacts?

QR Codes and Data Matrix codes can encode [contact information](http://zxing.appspot.com/generator). Upon scanning such a code, you will be prompted to add the contact information to your contacts list. In addition, Barcode Scanner can encode a contact as a QR Code and present it on-screen, so that a friend can point their phone at your screen to transfer that contact. This is why the app requests permission to write and read contacts. They are not used in any other way.

#### ... my browser history and bookmarks?

Just like contacts, you can share a bookmark with a friend by [encoding the URL](http://zxing.appspot.com/generator) as a QR Code. It is then shown on screen for him or her to scan with their phone. That's the only thing we use them for.

#### ... my SD card?

You can use Barcode Scanner to email a generated QR code or your scan history. To do this, the barcode image / history CSV file must first be written to device storage, which is why the permission is needed. As a bonus, this makes this output available for retrieval from your SD card directly. It is not used for any other purpose.

#### ... my wi-fi settings?

QR codes can [encode the user name, password, etc. for a wi-fi network](http://zxing.appspot.com/generator) so that you don't have to type them into your phone. To do this, the application needs to be able to change wi-fi settings.

#### ... the Internet?

A few types of scan results will trigger an action to retrieve more information about the result. For example, URL redirectors will be resolved, and web pages will be retrieved up to the title, to give some clue about the destination of the encoded URL before accessing. This needs internet access. Likewise, product and book barcodes will cause a lookup of additional detail about the product or book from internet resources. These can be disabled in settings.

### It doesn't focus or scan on my phone

Most reports of things like this are unfortunately traceable to camera driver problems. This can be true even if other apps like the camera work. The problem is of the form:

* Barcode Scanner app asks device if it supports feature X
* Devices reports it supports X
* Barcode Scanner requests feature X
* Camera driver does not support it correctly, and fails to initialize
* *All* initialization including light, focus and other settings fail, making scanning hard or impossible

**First**, try disabling camera features in Settings. Look for "Device Bug Workarounds". Usually, disabling one or more of these will work around the issue.

If not, only report an issue like this on the mailing list if you can also supply log output from the app using `adb logcat` or the [aLogcat app](https://play.google.com/store/apps/details?id=org.jtb.alogcat). Find log messages from the time the app first opens, especially including any apparent exceptions or error messages. This is useful information even if it does not lead to a workaround. Ideally, provide a proposed code change.

### Barcode Scanner sets off a virus warning

The Barcode Scanner app contains no viruses or malware. 

To ensure you get the official copy from this project, only download Barcode Scanner from [Google Play](https://play.google.com/store/apps/details?id=com.google.zxing.client.android).

This is built and signed by us, and Google Play uses these signatures to verify that the copy you get is from us. Other app stores, including Amazon, may have a copy of the Barcode Scanner app, but these are not built or maintained by us.

Worse still, because this is an open source project, it's easy for a malicious person to add viruses or malware, build and distributed a look-alike copy. For this reason, don't obtain copies from third-party app stores.

### Can we distribute Barcode Scanner, or pre-install it on our Android devices?

Yes. The application is distributed under the terms of the [Apache License 2.0](http://www.apache.org/licenses/LICENSE-2.0.html). Distributing the .apk files released by this project complies with the terms of the license, with no further action required. There is no fee. These are the only available license terms.

We *strongly* recommend distributing the project's release, rather than building from source, for best user experience. It is signed with the project's keys and so is the only copy that will be upgradeable to future releases via Google Play.

The project only maintains a release of Barcode Scanner on [Google Play](https://play.google.com/store/apps/details?id=com.google.zxing.client.android) and does not maintain releases elsewhere. Other app stores are however welcome to maintain releases on their own.

### Can we talk about monetizing Barcode Scanner using our ad network, or paid promotion?

The app has always been ad-free and has not been monetized, and will continue to be so. Barcode Scanner will not run ads or otherwise monetize. It is the work of many owners and would be difficult to fairly allocate revenue. It is open source and could be built without ads easily anyway.

As a result there is no interest in advertising or paying to promote the app either.

### I don't get prices from Barcode Scanner, or get very high prices, for some products

This application does not maintain or provide any product information, so these are not problems with Barcode Scanner itself. It links to Google Product Search and Web Search; contact Google with suggestions.

Note that common household items are often available for purchase online, but in bulk quantity. This is one reason that results for the product's price seem high: they are not for individual items. Again, this is not an issue with Barcode Scanner itself.

### The app won't download or install 

These are functions of Google Play, and outside the application's control. We do get some reports of stuck downloads which can be fixed by rebooting the phone. If that doesn't help, please contact the phone manufacturer or your carrier.

### I can't uninstall the Barcode Scanner application 

Some manufacturers put Barcode Scanner into the "firmware" of the phone, which means it is not uninstallable. This is an issue to take up with manufacturers or carriers. Sorry, we can't do anything about this.

### Does Barcode Scanner send messages to my contacts?

No, absolutely not. I have seen reports of user's contacts receiving messages like:

> I've been using Barcode Scanner and I think you might like it. Check it out from your Android phone:
> http://market.android.com/search?q=pname:com.google.zxing.client.android
> *Note: Tap the Market icon in the Complete action screen that appears after you tap the link above.

These are not sent by Barcode Scanner. In fact, you can see instances of this message pertaining to many other applications by [searching Google](https://www.google.com/search?q='and+I+think+you+might+like+it.+Check+it+out+from+your+Android+phone').

One [forum post suggests](http://www.incredibleforum.com/forum/new-member-introductions-site-assistance/5365-help-mass-emails-being-sent-my-incredible.html) that these are being send by an HTC app called "App Sharing".