## Just Need a JAR?

If you just need a *pre-built JAR file*, you can always find compiled resources from the [Maven release repository](http://repo1.maven.org/maven2/com/google/zxing/), including [recent snapshot/nightly builds](https://oss.sonatype.org/content/repositories/snapshots/com/google/zxing/).

## Download

Download the [latest release](https://github.com/zxing/zxing/releases), version 3.0.0 or later. Or, retrieve the latest source code from [Github](https://github.com/zxing/zxing).

The code is organized into several subdirectories, corresponding to modules, like `core/` and `javase/`. Within each Java-based module, there is a `pom.xml` file for use with [Apache Maven](http://maven.apache.org/). 

## Configure

A few configuration steps are needed, depending on which modules you want to build. From the directory where you unpacked or checked out the source code:

### android/

The Android SDK must be installed of course. Run the tool called `android` and ensure that platform support for the latest Android release is installed. At the time of this writing, that's platform level 19 / Android 4.4.

#### Release 3.0.0 and earlier

```
android update project --path android
```
or
```
echo "sdk.dir=/change/this/path/to/android-sdk" > android/local.properties
```

#### Release 3.0.1 and later

```
export ANDROID_HOME=/change/this/path/to/android-sdk
```

## Build

### android/

#### Release 3.0.0 and earlier

1. Build `core/` below, first
1. Run `ant debug` from `android/` to build the Barcode Scanner application as `bin/Barcode Scanner-debug.apk`.

#### Release 3.1.0 and later

From `android/`, run `mvn package android:apk` to produce a compile `.apk` file in `target`. Use `android-x.y.z-aligned.apk`.

Other users will not be able to build the signed release version, but the command is: `mvn -Pandroid-release -Djarsigner.storepass=... -Djarsigner.keypass=... clean package android:apk`.

### android-integration/

From `android-integration/`, run `mvn package` to produce compiled libraries like `android-integration-x.y.z.jar` in the `target/` directory.

### core/

From `core/`, run `mvn -DskipTests package` to product compiled libraries like `core-x.y.z.jar` in the `target/` directory.

### javase/

From `javase/`, run `mvn package` to product compiled libraries like `javase-x.y.z.jar` in the `target/` directory.

### zxingorg/

From `zxingorg/`, run `mvn package` to produce a `.war` in the `target/` directory.

## Run

_Most components are libraries and are not run directly._

### android/

1. Build `android/`
1. Connect your device via USB
1. If you already have the standard version of Barcode Scanner installed, uninstall it
1. Make sure your device is set to allow apps from untrusted sources
1. In release 3.0.0 and earlier, run `ant installd` to install the debug build. In release 3.1.0 and later, run `mvn android:deploy`.

### javase/

After building, simply run this class with `java` from the top-level directory:

```
java -cp javase/target/javase-x.y.z.jar:core/target/core-x.y.z.jar com.google.zxing.client.j2se.CommandLineRunner [URL | FILE]
```

_Path syntax is different on Windows. Here and elsewhere you will need to use ';' rather than ':' to separate classpath elements, and likely need to use the '\' path separator._

## Maven

`core/`, `javase/`, `android-integration` and `zxingorg` can be used directly in a Maven-based project without any download or installation. Instead, add as dependencies from groupID `com.google.zxing` artifactIDs `core`, `javase`, `android-integration` or `zxingorg`:

```
<dependencies>
  ...
  <dependency>
    <groupId>com.google.zxing</groupId>
    <artifactId>core</artifactId>
    <version>3.0.0</version>
  </dependency>
</dependencies>
```