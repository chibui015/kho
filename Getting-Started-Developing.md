## Just Need a JAR?

If you just need a *pre-built JAR file*, you can always find compiled resources from the [Maven release repository](http://repo1.maven.org/maven2/com/google/zxing/), including [recent snapshot/nightly builds](https://oss.sonatype.org/content/repositories/snapshots/com/google/zxing/).

## Download

Download the [latest release](https://github.com/zxing/zxing/releases), version 3.0.0 or later. Or, retrieve the latest source code from [Github](https://github.com/zxing/zxing).

The code is organized into several subdirectories, corresponding to modules, like `core/` and `javase/`. Within each Java-based module, there is a `pom.xml` file for use with [Apache Maven](http://maven.apache.org/). 

## Configure

A few configuration steps are needed, depending on which modules you want to build. From the directory where you unpacked or checked out the source code:

### android/

The Android SDK must be installed of course. Run the tool called `android` and ensure that platform support for the latest Android release is installed. At the time of this writing, that's platform level 21 / Android 5.0.1.

```
export ANDROID_HOME=/change/this/path/to/android-sdk
```

## Build

From the root of the project, run `mvn package` to compile, test and assemble all modules. Add `-DskipTests` to skip unit tests. Note that Android-related modules and apps will not be built unless `ANDROID_HOME` is set.

Compiled `.jar` files are found in submodules after this. For example, the compiled `core/` code is available at `core/target/core-x.y.z.jar`.

### android/

To build the Barcode Scanner Android app, a few slightly different steps are needed. From `android/`, run `mvn package android:apk` to produce a compile `.apk` file in `target`. Use `android-x.y.z-aligned.apk`.

Other users will not be able to build the signed release version, but the command is: `mvn -Pandroid-release -Djarsigner.storepass=... -Djarsigner.keypass=... clean package android:apk`.

### androidtest/ glass/

Being Android apps as well, the ZXing Test and Barcode Scanner for Google Glass application APKs are built similarly to `android/`.

### zxingorg/

Note that the deployable `.war` file will be produced in the `target/` directory.

## Run

_Most components are libraries and are not run directly._

### android/

1. Build `android/`
1. Connect your device via USB
1. If you already have the standard version of Barcode Scanner installed, uninstall it
1. Make sure your device is set to allow apps from untrusted sources
1. In release 3.0.0 and earlier, run `ant installd` to install the debug build. In release 3.0.1 and later, run `mvn android:deploy`.

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
    <version>(the current version)</version>
  </dependency>
</dependencies>
```

## Project Developers Only: Release Process

1. For the three apps (`android`, `androidtest`, `glass`), update their `parent` version to be the upcoming release and commit locally
1. Make sure `CHANGES` is up to date and everything is committed
1. Update the current milestone in Github as needed and close it
1. `unset ANDROID_HOME` so as to not release Android apps
1. `mvn -s private-settings.xml clean release:clean release:prepare` and optionally add `-DreleaseVersion=x.y.z -DdevelopmentVersion=a.b.c-SNAPSHOT` if the answer is not the one Maven guesses and you want to avoid repeating the answer
1. If all is well, `mvn -Darguments="-Dgpg.passphrase=..." -s private-settings.xml release:perform`
1. Log in to `oss.sonatype.org` and finish the release (http://central.sonatype.org/pages/releasing-the-deployment.html).
1. Update the release on Github
1. Announce the release on the mailing list
1. To immediately publish a next snapshot, `mvn -s private-settings.xml clean deploy`
1. To get the site ready, first go back to the tag, `git checkout -f tags/zxing-x.y.z`
1. `mvn clean site:site site:stage`
1. In a temp directory somewhere, `git clone -b gh-pages https://github.com/zxing/zxing.git`
1. `rm -r [temp]/*`
1. `cp -r [zxing dir]/target/staging/* [temp]/`
1. In that temp dir, `git add -A .`
1. `git commit -m 'Update site for x.y.z'`
1. `git push origin gh-pages`
1. Delete the temp dir
1. Sync with `git checkout -f master` and `git pull`