---
# video metadata defined in _layouts/tour.html
layout: tour
title: Getting Started with Skip Video
---

<!--
## Transcript

Welcome to Getting Started with Skip.

Skip is a technology for creating dual-platform native iOS and Android apps using Swift and SwiftUI.

For more information on Skip, visit our website at skip.tools.

This video is going to walk you through installing Skip and creating your first dual-platform Skip app.

Skip requires a macOS machine with the latest Xcode, Android Studio, and Homebrew installed.

Xcode is Apple's integrated development environment for building Swift and SwiftUI apps, and that's where you'll spend most of your time when developing with Skip.

You can get Xcode from the Mac App Store or at developer.apple.com/xcode.

Android Studio is an integrated development environment for Android, and you'll need it to run the Android version of your app.

You can download Android Studio at developer.android.com/studio.

Finally, Homebrew is a popular package manager for macOS X.

You'll use this to install and update the Skip command line tool.

Visit brew.sh for installation instructions.

Once you have these three pieces of software installed, you're ready to install Skip.

Open Terminal and use Homebrew to install the Skip command line tool.

This will download and install the Skip tool itself, as well as the Gradle build system for Android and any JDK dependencies that are necessary for building and testing the Kotlin side of your apps.

Once that's complete, we can ensure that all development prerequisites are met by running Skip checkup.

This might take some time because in order to make sure everything is working, Skip performs actual iOS and Android builds of a small app.

And the first time you do an Android build, Gradle may have to download hundreds of megabytes of Java and Kotlin dependencies.

Don't worry, this is a normal part of building apps for Android.

It only happens the first time, and the downloaded libraries don't bloat your Android app.

They're just needed for development.

If the checkup passes, you're ready to create your first Skip app.

If you see any failures, try passing the -v argument to Skip to get verbose output.

To create an app, we use the skip init command.

We give it your app's bundle ID, project name, and app name.

This creates a HelloSkip folder for our app.

The Xcode project will be in the Darwin subfolder.

Let's open it.

And already, we're almost ready to run your app.

But first, we need to be running an Android emulator.

The easiest way to run an emulator is from Android Studio.

We open up the Virtual Device Manager and start one of the emulators from there.

With that done, let's set a simulator target in Xcode and run your app.

Congratulations!

You're now running your first dual-platform app written in Swift and SwiftUI.

You'll notice that the UI isn't exactly the same across platforms.

Skip creates fully native apps, which means it's using your SwiftUI unaltered on iOS and creating a Jetpack Compose UI on Android.

This sample UI is contained in the content view Swift file.

And we can see that it's stock SwiftUI as you'd expect in any iOS app.

But now with Skip, it's powering your Android version as well.

Now that you're up and running, we encourage you to play around.

Maybe make some modifications to the source.

[ Applause ] Okay.

So, this is the first time we've seen Skip.

Here we've added a slider to rotate our initial UI.

It's magical to see your Swift and SwiftUI code come to life on Android.

But of course, Skip is not actual magic.

It's a tool that you work with, and the docs do a good job of covering common issues.

You can even read about how you can contribute to Skip if you'd like to expand its capabilities.

And if you have any problems, we are here to help.

Your feedback is extremely valuable to us.

Be sure to check out our website covering various aspects of Skip development.

But for now, happy skipping.
-->
