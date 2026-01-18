---
# video metadata defined in _layouts/tour.html
layout: tour
title: Skip Introduction Video
---

<!-- This is a developer's introduction to Skip, a technology for creating dual-platform iOS and Android apps using Swift and SwiftUI. After taking the video tour, [get started](/docs/gettingstarted) building your own dual-platform app. -->


## Transcript

[<a href="#" onclick="jumpToTime('0:00')">0:00</a>] Introducing Skip, a new technology for iOS and Android mobile app development.
Let’s dive right in and hit the run button in Xcode.

[<a href="#" onclick="jumpToTime('0:15')">0:15</a>] We’re now running the Skip sample Weather app.
We’ve launched the app on both iOS and Android, and we’ll talk about Android shortly. But for now, you’ve probably already noticed that the app doesn’t look exactly the same on both platforms.
Skip creates fully native apps, which includes using the native UI components of each OS. That means SwiftUI on iOS, and Compose on Android. In fact, you can see our main SwiftUI view here, which sets up the content of the app’s tabs.

[<a href="#" onclick="jumpToTime('0:48')">0:48</a>] And here is the SwiftUI ListView displaying this list of cities.
Tapping a city pushes the WeatherView to display that city’s current weather.

[<a href="#" onclick="jumpToTime('1:12')">1:12</a>] The WeatherView uses an observable WeatherCondition to fetch the latest weather.
WeatherCondition has observable properties, and it uses an async function to retrieve the weather.

[<a href="#" onclick="jumpToTime('1:33')">1:33</a>] So that was a whirlwind tour of some of our iOS code.
There shouldn’t have been any surprises there - this is a pretty standard looking app using standard Swift and SwiftUI.
What is not standard, is that all of the iOS Swift and SwiftUI code we just saw is powering our Android app as well.

[<a href="#" onclick="jumpToTime('1:52')">1:52</a>] Let’s prove it. When the WeatherView displays the temperature, let’s have it display a message too.
And let’s ask Xcode to build and run…

[<a href="#" onclick="jumpToTime('2:40')">2:40</a>] And there’s our new addition to the weather view, running on both platforms.
As you just saw, iterating on your dual-platform Skip app with Xcode is just like iterating on your iOS app.
But if you’re being a conscientious developer, you’ll have unit tests as well, and we have some model tests.

[<a href="#" onclick="jumpToTime('3:02')">3:02</a>] Skip has a surprise for us here as well.
If we run our unit tests using the Mac, Skip will also run our tests on Android using Roboelectric, which is a tool to run Android tests on your computer.

[<a href="#" onclick="jumpToTime('3:17')">3:17</a>] It’ll take a minute to recompile everything for Mac, but we think it’s worth showing off. 
Because while unit testing doesn’t make for a flashy demo, we can’t overstate how nice it is to know that your logic is passing the same tests on both platforms.

[<a href="#" onclick="jumpToTime('3:34')">3:34</a>] If we watch the output we can see that the tests are run for iOS first, and then skip launches Android’s build tool Gradle to test with Roboelectric.

[<a href="#" onclick="jumpToTime('4:04')">4:04</a>] Now let’s introduce a failure case to see what happens.
I’ll note that the Android test run is modeled as just one more unit test. So when you set up your continuous integration system to run your tests, it will include the Android tests automatically.
We do this ourselves in our own CI runs.

[<a href="#" onclick="jumpToTime('4:24')">4:24</a>] Now the failure has been noted for iOS. And the Android tests should run in a moment…
Now we see that the test failed on Android as well. We can see the details of both failures right here in Xcode.

[<a href="#" onclick="jumpToTime('4:40')">4:40</a>] OK, so what is happening?
We have an iOS app, with iOS unit tests, using Xcode, and now it’s all running on Android.
It feels like we “skipped” something.

[<a href="#" onclick="jumpToTime('4:54')">4:54</a>] Well, Skip is not somehow emulating iOS on Android.
Instead, let’s see what Skip is doing.

[<a href="#" onclick="jumpToTime('5:02')">5:02</a>] Skip is actually a number of things.
One is that it’s a Swift Package Manager plugin - we’ve seen it in action already, running through Xcode.

[<a href="#" onclick="jumpToTime('5:13')">5:13</a>] Another is that it’s an intelligent Swift to Kotlin transpiler. 
The transpiler uses Apple’s SwiftSyntax library to parse your Swift code - the same library used by the new Swift macros - and generates nice, human-readable Kotlin code.

[<a href="#" onclick="jumpToTime('5:33')">5:33</a>] We have the Swift code we wrote on the left, and here is the corresponding Kotlin that Skip generated.
The Swift code is using async, and the Kotlin is using a suspend function.
The Swift is confined to the main actor, so the generated Kotlin runs on the main thread too.

[<a href="#" onclick="jumpToTime('6:13')">6:13</a>] If you know Kotlin, most of this should look familiar.
But you might be wondering how Kotlin is using iOS Foundation API like URLSession and JSONDecoder.

[<a href="#" onclick="jumpToTime('6:38')">6:38</a>] Skip is also a set of Android libraries. 
The core libraries reimplement iOS API from app building blocks like the Swift standard library and Foundation, but targeting Kotlin and Android.
We also have a SkipUI library that mirrors SwiftUI on top of Jetpack Compose, which is Android’s declarative UI framework.

[<a href="#" onclick="jumpToTime('7:02')">7:02</a>] And that assertion error we received when we created a failing unit test? That was courtesy of the SkipUnit library, which converts your XCTest cases into JUnit, which is the standard Java and Kotlin unit testing framework.

[<a href="#" onclick="jumpToTime('7:19')">7:19</a>] Skip’s transpiler is proprietary, but all of these libraries are free and open source, and we welcome contributions.

[<a href="#" onclick="jumpToTime('7:28')">7:28</a>] So Skip uses intelligent transpilation plus a set of open source libraries to create your Android app. 
What are the advantages of this approach to dual-platform development?

[<a href="#" onclick="jumpToTime('7:40')">7:40</a>] First, the fact that Skip uses transpilation  means there is no hidden runtime environment on either platform.
The iOS app is a pure native iOS app with zero dependencies on Skip. You didn’t see any Skip imports in the code because there aren’t any. We call this transparent adoption, and it’s a unique aspect of Skip.

[<a href="#" onclick="jumpToTime('8:04')">8:04</a>] This sample Weather app distribution is just kilobytes in size on iOS.

[<a href="#" onclick="jumpToTime('8:10')">8:10</a>] The transpiled Android app is also a fully native Kotlin and Compose app.
It does use Skip’s Foundation and SkipUI libraries and others, but it isn’t running *through* them. They just mirror Swift API for us. The app is running fully native Kotlin and Compose.

[<a href="#" onclick="jumpToTime('8:31')">8:31</a>] That means fully native look and feel, and fully native performance on both platforms.
It means no additional memory overhead that’s going to get your app killed by the OS.
And transpilation, combined with the fact that Skip’s libraries are fully open source means that you have access to every bit of the code. Nothing in your app is hidden from you or just as importantly from your debugger.

[<a href="#" onclick="jumpToTime('8:59')">8:59</a>] In fact here I’ve opened the generated Android app right in Android Studio.
Here’s the transpilation of our Weather model again.

[<a href="#" onclick="jumpToTime('9:15')">9:15</a>] So what’s the debugging story in Skip?
Well if you’re trying to track down a general logic bug, you have access to the full array of Xcode debugging tools, and you can just debug the iOS side of the app. The changes you make will of course fix the bug for the Android logic as well.
And you can always write unit tests to help.

[<a href="#" onclick="jumpToTime('9:38')">9:38</a>] But if it’s an Android-specific bug, you can run the app right in Android Studio and utilize the full power of its own debugging tools. You get the native debugging experience on both platforms.

[<a href="#" onclick="jumpToTime('9:53')">9:53</a>] That includes being able to dig into Skip’s own source code.
For example, here is the Kotlin for the SkipUI library’s VStack implementation.
We can see that it ends up using the Compose “Column” component under the hood.

[<a href="#" onclick="jumpToTime('10:10')">10:10</a>] One interesting note is that VStack, like most of Skip’s library code, is actually written in Swift. What we’re seeing is its transpilation.
Let’s look at its Swift source code to see how we can call Kotlin and Compose API from Swift.

[<a href="#" onclick="jumpToTime('10:32')">10:32</a>] So what probably jumps out at you right away is this #if SKIP block at the top.
This is one of several ways that we have of writing Android-specific code. Anything in the SKIP block will be transpiled by Skip, but is invisible to the Swift compiler. It has to be valid Swift code, but you can import Kotlin packages like we’re doing here.
And you can use Kotlin and Java APIs right inline.

[<a href="#" onclick="jumpToTime('11:00')">11:00</a>] Skip has other mechanisms too, including being able to add pure Kotlin files to your Xcode project and have them automatically included in your Android build.

[<a href="#" onclick="jumpToTime('11:10')">11:10</a>] So another advantage of Skip’s approach is that integrating with other Kotlin and Java and Compose code is natural. It doesn’t require any complicated bridging.

[<a href="#" onclick="jumpToTime('11:22')">11:22</a>] So that is Skip.
We’ll end this with some SwiftUI, as only Skip can.

[<a href="#" onclick="jumpToTime('11:31')">11:31</a>] With Skip, you create an iOS app using Swift and SwiftUI. You work in Xcode.
Skip acts like your Android team, creating the equivalent Android app on the fly.
Use one codebase to deliver fully native apps for both iOS *and* Android, unencumbered by added runtimes, garbage collectors, and non-native components.

[<a href="#" onclick="jumpToTime('11:55')">11:55</a>] Skip is new, but we’re moving fast.
To get more information on its current status, to try Skip out, and to see how you can get involved in the Skip community, visit our website at [https://skip.tools](https://skip.tools).


