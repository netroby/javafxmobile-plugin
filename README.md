# javafxmobile-plugin [![Build Status](https://travis-ci.org/javafxports/javafxmobile-plugin.svg?branch=master)](https://travis-ci.org/javafxports/javafxmobile-plugin) [![Maven Central](https://maven-badges.herokuapp.com/maven-central/org.javafxports/jfxmobile-plugin/badge.svg)](https://maven-badges.herokuapp.com/maven-central/org.javafxports/jfxmobile-plugin)

The javafxmobile-plugin is a gradle plugin that unifies the building of Java and JavaFX
applications for different target platforms:

* desktop
* android
* ios

## Before you start

[javafxmobile-plugin version 1.x](https://bitbucket.org/javafxports/javafxmobile-plugin) brought
Java 8 to mobile development. Version 2.x is an upgrade of the plugin that enables Java 9
development by leveraging [Gluon VM](https://gluonhq.com/products/mobile/vm/). Gluon VM is
Gluon's custom developed Java virtual machine that specifically targets mobile devices. It is
still in active development and is at the moment only supported on iOS devices.

The compatibility matrix below can help you in deciding if you should use version 1 or 2 of the
plugin.

Platform | JDK | Plugin
-------- | --- | -------
iOS | 8 | 1
Android | 8 | 1
iOS | 9 | 2
Android | 9 | 2 (very experimental <sup>[1](#fn1)</sup>)

## Getting started

### IDE plugin

The easiest way to get started is by using the Gluon IDE plugin for your IDE:

* [NetBeans](http://plugins.netbeans.org/plugin/57602/gluon-plugin)
* [IntelliJ IDEA](https://plugins.jetbrains.com/plugin/7864-gluon-plugin)
* [Eclipse](https://marketplace.eclipse.org/content/gluon-plugin)

Once the IDE plugin is installed, you can create a new Gluon Mobile project.

### Sample

A complete basic sample is also available from the Gluon samples repository:

https://github.com/gluonhq/gluon-samples/tree/master/singleview-gluonvm

## Prerequisites

### General

1. [JDK 9](http://www.oracle.com/technetwork/java/javase/downloads/jdk9-downloads-3848520.html)
2. [Gradle 4.2+](https://gradle.org)

### iOS

1. A Mac with MacOS X 10.13.2 or superior
2. Xcode 9.2 or superior, available from the Mac App Store
3. To deploy on an iOS device, the `usbmuxd` library, a cross-platform software library that talks the protocols to support iOS devices. Install it with the use of [Homebrew](https://brew.sh):

    ```
    brew install usbmuxd
    ``` 

4. It is highly recommended to increase the Java stack size, by adding the following option to the `~/.gradle/gradle.properties` file:
 
    ```
    org.gradle.jvmargs=-Xms256m -Xmx4096m -Xss2m
    ```

Be aware that the first time the plugin runs an iOS task, it will take a long time (more than 15 minutes) to ahead-of-time compile all the Java modules. 
After this finishes successfully, all these modules are cached locally (`~/.gvm/aot`), so the next runs will be shorter (just a few minutes).

### Android

<b name="fn1">Experimental!</b>

Be aware that Java 9 support on Android is very experimental. You can only use Java 9 APIs as
long as you remain backward compatible with Java 8. For instance, you can not use custom skins
of JavaFX controls, because those APIs were changed completely in JavaFX 9.

1. The Android SDK command line tools, available [here](https://developer.android.com/studio/index.html#command-tools)
2. Use the `sdkmanager` command line tool to install the following packages:

    ```
    ANDROID_SDK/tools/bin/sdkmanager "platform-tools" "build-tools;27.0.3" "platforms;android-25" "extras;android;m2repository" "extras;google;m2repository"
    ```

3. Define a global gradle property named ANDROID_HOME inside ~/.gradle/gradle.properties that
points to the location of the Android SDK:

    ```
    ANDROID_HOME=/path/to/android-sdk-directory
    ```

4. Make sure to set the source and target compatibility in build.gradle to Java 8 to make your
applications deployable on Android.

    ```
    sourceCompatibility = 1.8
    targetCompatibility = 1.8
    ```

## Usage

If you create your project without the use of the IDE plugins, add the following to your
`build.gradle`:

```
buildscript {
    repositories {
        jcenter()
        google()
        maven {
            url 'http://nexus.gluonhq.com/nexus/content/repositories/releases'
        }
    }
    dependencies {
        classpath 'org.javafxports:jfxmobile-plugin:2.0.0'
    }
}

apply plugin: 'org.javafxports.jfxmobile'
```

## Java 9

Full Java 9 support is in an experimental phase and is currently only supported on iOS.

## License

This project is licensed under the [3-Clause BSD license](https://opensource.org/licenses/BSD-3-Clause).
