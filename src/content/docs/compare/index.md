---
title: Skip Comparison Matrix
permalink: /compare/
tableOfContents: true
---

<style>
table {
    font-family: system-ui;
    border-collapse: collapse;
    width: 100%;
    margin: 0 auto;
    /* settings so the table has its own scroll bar and doesn't make the page wider */
    overflow-x: auto;
    max-width: fit-content;
    display: block;
}

th, td {
    text-align: left;
    padding: 12px;
    font-size: 70%;
}

th {
    background-color: var(--sl-color-accent);
    color: var(--sl-color-black);
    min-width: 55pt;
}

td:nth-child(1) {
    /* font-weight: 400; */
}

/* Highlight the second "Skip" column */
td:nth-child(2999) {
    /* background-color: var(--sl-color-gray-4); */
    /* color: var(--text-background-color); */
    font-weight: bolder;
}

tr:nth-child(even) {
    background-color: var(--sl-color-gray-6);
}

tr:hover {
    background-color: var(--sl-color-gray-7);
}

td {
    border: 1px solid var(--sl-color-gray-5);
    font-weight: normal;
}

h1 {
    text-align: center;
    padding: 1em;
}

img.table-icon {
    width: 22pt;
    height: 22pt;
    padding: 10px;
}
</style>

How does Skip compare to other cross-platform frameworks? The table below evaluates the criteria that matter most for production mobile apps: native UI, performance, platform API access, and long-term maintainability.

| | Write<br/>Twice | Skip | Flutter | React<br/>Native | Compose<br/>Multiplatform | |
|-:|:-:|:-:|:-:|:-:|:-:|:-|
| Develop with a single language<br/>in a single codebase | ❌ | ✅ | ✅ | ✅ | ✅ |
| Modern memory-safe language | ✅ | ✅  | ✅  | ✅ | ✅ |
| Natively Compiled | ✅ | ✅ | ✅ | ❌ | ✅ | |
| Memory efficient<br/>(no added garbage collection) | ✅<br/>✅ | ✅<br/>✅ | ❌<br/>❌ | ❌<br/>❌ | ❌<br/>✅ | iOS<br/>Android |
| Platform-native widgets | ✅<br/>✅ | ✅<br/>✅  | ❌<br/>❌ | ✅<br/>✅ | ❌<br/>✅ | iOS: native UIKit<br/>Android: native Views |
| Vendor-recommended toolkit | ✅<br/>✅ | ✅<br/>✅  | ❌<br/>❌ | ❌<br/>❌ | ❌<br/>✅ | Apple: SwiftUI<br/>Google: Jetpack Compose |
| Effortless platform API access | ✅<br/>✅ | ✅<br/>✅ | ❌<br/>❌ | ❌<br/>❌ | ❌<br/>✅ | iOS: Swift<br/>Android: Kotlin |
| Ejectable | N/A | ✅ | ❌ | ❌ | ❌ |


# Technology Comparison Table

The following table breaks down the underlying technology stack of each framework.

|                         | Skip       | Flutter          | React Native | Xamarin          | KMP Compose      | Ionic/Cordova |
|------------------------:|:-----------|:-----------------|:-------------|:-----------------|:-----------------|:--------------|
| Development Language    | Swift      | Dart             | JavaScript   | C#               | Kotlin           | JavaScript    |
| UI Framework            | SwiftUI    | Flutter          | HTML/JSX+CSS | XAML/MAUI        | Compose          | HTML+CSS      |
| Package Manager         | SwiftPM    | Pub/CocoaPods    | NPM          | NuGet/CocoaPods  | Gradle/CocoaPods | Varies        |
| IDE                     | Xcode      | Android Studio   | VSCode       | Visual Studio    | IntelliJ IDEA    | VSCode        |
| iOS UX                  | Native     | Mimicked         | Native       | Native           | Mimicked         | HTML          |
| Android UX              | Native     | Mimicked         | Native       | Native           | Native           | HTML          |
| Rendering Canvas        | Native     | Impeller/Skia    | Flexbox      | Native           | Skia             | WebView       |
| Code Execution          | Compiled   | Compiled         | Interpreted  | Compiled         | Compiled         | Interpreted   |
| Call Platform API       | Direct     | Bridged          | Bridged      | Bridged          | Mixed            | Bridged       |
| Added VM/Runtime        | None       | Flutter Engine   | Hermes       | Mono             | Kotlin/Native    | Varies        |
| Added Garbage Collector | None       | Yes              | Yes          | Yes              | Yes              | Yes           |


---

## Detailed Framework Comparisons

The tables above provide a summary view. The sections below examine each major alternative in depth, drawing on independent benchmarks, production post-mortems, and analyst assessments to illustrate where these frameworks fall short of what production mobile teams actually need.

### Skip vs. Flutter

Flutter is Google's cross-platform UI toolkit, built around the Dart programming language and a custom rendering engine. It is the most widely adopted cross-platform framework by developer count, and it has genuine strengths: a productive hot-reload cycle, a large widget library, and a single codebase that compiles for multiple platforms. However, Flutter's architectural decisions introduce a set of trade-offs that become increasingly costly as apps mature and platform expectations evolve.

#### Flutter does not use native UI components

Flutter's defining architectural choice is to bypass the platform's native UI toolkit entirely. Rather than rendering with UIKit or SwiftUI on iOS, or with Android Views or Jetpack Compose on Android, Flutter draws every pixel through its own rendering engine (Skia, or its successor Impeller). The result is a UI that approximates the look of each platform but does not actually use the platform's own components.

This creates what the industry has taken to calling the ["uncanny valley"](https://medium.com/serverpod/is-it-time-for-flutter-to-leave-the-uncanny-valley-b7f2cdb834ae) problem: Flutter apps look close to native, but not quite right. Scrolling physics, text selection behavior, context menus, and system-level integrations such as accessibility semantics all differ subtly from what users expect on each platform. These differences compound. Users may not articulate why a Flutter app feels "off," but the aggregate effect is a lower-quality experience compared to apps built with the platform's own toolkit.

The consequences of this approach became sharply visible with the release of iOS 26 and Apple's "Liquid Glass" design language. Apps built with SwiftUI adopted the new visual style automatically. The Flutter team, however, [confirmed](https://github.com/flutter/flutter/issues/170310) that they are "not developing the new Apple '26 UI design features in the Cupertino library right now, and we will not be accepting contributions for these updates at this time." Flutter apps on iOS 26 will look visually dated until the Flutter team reimplements Apple's design changes from scratch, a process with no announced timeline. As [one analysis noted](https://iamvishnu.com/posts/liquid-glass-and-flutter), the cost of conditionally rendering platform-appropriate UI on both iOS and Android in Flutter approaches the cost of simply writing native apps in the first place.

Skip apps, by contrast, use SwiftUI on iOS and Jetpack Compose on Android. There is no reimplementation step. When Apple or Google updates their design language, Skip apps inherit those changes the same way any native app does.

#### Performance overhead from the Dart runtime and garbage collector

Flutter bundles the Dart virtual machine and its garbage collector into every app. Independent measurements from [Flutter's own issue tracker](https://github.com/flutter/flutter/issues/48391) show that a minimal Flutter app consumes roughly 40 MB of memory at launch, compared to approximately 20 MB for an equivalent native Android app. Graphics memory usage runs at 2.3x the native baseline, and native library overhead is 6x higher. [AltexSoft's analysis](https://www.altexsoft.com/blog/pros-and-cons-of-flutter-app-development/) found that Flutter uses approximately 50% more memory than even React Native.

The Dart garbage collector poses a particular problem for fluid UI. A [long-standing Dart SDK issue](https://github.com/dart-lang/sdk/issues/47337) documents frequent and lengthy garbage collection pauses during scrolling, sometimes triggered on every finger swipe over basic widgets. The problem is structural: Flutter's widget-heavy architecture creates large volumes of short-lived objects that the GC must constantly reclaim. On devices with 90 Hz or 120 Hz displays, where frame budgets are 11 ms or 8 ms respectively, even brief GC pauses cause visible stuttering.

Skip adds no runtime or garbage collector to your iOS app. On Android, Skip Fuse compiles Swift natively, and Skip Lite produces standard Kotlin that runs on the platform's own runtime. In both cases, there is no additional GC layer competing with the UI thread for time.

#### App size inflation

A minimal Flutter app weighs 15 to 20 MB, compared to 2 to 4 MB for native Android and 1 to 3 MB for native iOS. This overhead comes from bundling the rendering engine, the Dart runtime, and the International Components for Unicode (ICU) library. A feature-rich Flutter app [typically reaches 25 to 35 MB](https://medium.com/@naufalprakoso24/why-flutter-apps-are-larger-than-you-think-and-its-not-your-images-248fb3c22158) before any app-specific assets are added. By contrast, a Skip iOS app using SkipZero [can be as small as 25 KB](https://github.com/skiptools/skipapp-hello/releases), because the iOS build has zero dependency on Skip frameworks.

#### Platform continuity risk

In April 2024, Google [laid off staff from the Flutter and Dart teams](https://techcrunch.com/2024/05/01/google-lays-off-staff-from-flutter-dart-python-weeks-before-its-developer-conference/) weeks before Google I/O. Key engineers departed, including the primary implementer of the Impeller GPU backend. Google has a [well-documented history](https://killedbygoogle.com) of discontinuing products and frameworks, and Flutter generates no direct revenue for the company, making it vulnerable during cost-reduction cycles. Google reaffirmed its commitment at subsequent events, but the layoffs [injected uncertainty](https://www.omgubuntu.co.uk/2024/05/googles-flutter-team-layoffs-leave-ubuntu-devs-in-a-flap) into organizations that had invested heavily in the framework, including Canonical's Ubuntu desktop team.

ThoughtWorks Technology Radar placed Flutter in the ["Trial" ring in November 2019](https://www.thoughtworks.com/en-us/radar/languages-and-frameworks/flutter) and has not promoted it to "Adopt" in any subsequent edition. It no longer appears on the current Radar.

Skip is open source, community-funded, and fully ejectable. If you stop using Skip, your iOS app remains a pristine SwiftUI project and continues to work without modification.

---

### Skip vs. React Native

React Native, maintained by Meta, uses JavaScript (or TypeScript) and a bridge architecture to render native platform components. It has a large ecosystem, broad community adoption, and the practical benefit of reusing web development skills for mobile work. In practice, however, the framework introduces significant complexity, performance constraints, and long-term maintenance costs that become harder to manage as apps grow.

#### The JavaScript bridge is a structural bottleneck

React Native's architecture routes all communication between application logic and native UI through a bridge. In the legacy architecture, every piece of data crossing this bridge must be serialized to JSON and deserialized on the other side. This serialization overhead is not incidental; it is the central throughput constraint of the framework. Complex list views, animations, and data-heavy screens routinely saturate the bridge, producing dropped frames and sluggish interactions.

Meta has been working on a "New Architecture" (Fabric renderer, TurboModules, and the JSI interface) to mitigate these constraints. The migration, however, has proven painful even for well-resourced teams. [Shopify's engineering team documented](https://shopify.engineering/react-native-new-architecture) their experience migrating 40+ native modules: they observed up to 20% load time regressions on complex components, and their crash-free session rate initially dropped below their 99.95% target. The [legacy architecture was frozen in June 2025](https://github.com/reactwg/react-native-new-architecture/discussions/290), meaning teams that have not migrated are now on an unsupported path.

Skip has no bridge. On iOS, your SwiftUI code runs without any intermediary. On Android, Skip Lite produces standard Kotlin and Compose code that executes directly on the platform, and Skip Fuse compiles Swift natively. There is no serialization layer, no bridge overhead, and no architecture migration to plan for.

#### Memory overhead and garbage collection

React Native embeds a JavaScript engine (Hermes) and its associated garbage collector into every app. A [2025 cross-framework benchmark](https://www.synergyboat.com/blog/flutter-vs-react-native-vs-native-performance-benchmark-2025) measured memory consumption during realistic list scrolling: React Native used 45 MB of additional memory, compared to 10 MB for native Swift, a 4.5x difference. The standard deviation was also substantially higher (10.94 MB vs. 0.18 MB), indicating less predictable memory behavior under load.

[Testing by Walmart Labs](https://dev.to/anotherjsguy/react-native-memory-profiling-jsc-vs-v8-vs-hermes-1c76) showed that Hermes, while efficient at startup, crashed after loading approximately 920 items in a FlatList, reaching 556 MB of memory. The garbage collector's behavior under sustained load is unpredictable enough that [Shopify explicitly states](https://shopify.engineering/five-years-of-react-native-at-shopify) that "100% React Native should be an anti-goal" for their apps.

#### JavaScript is the wrong language for mobile-first teams

React Native requires developers to work in JavaScript or TypeScript. For iOS teams that already know Swift, and for Android teams that already know Kotlin, this means adopting an entirely separate language ecosystem, build toolchain (NPM, Metro bundler), and set of idioms. In practice, most React Native teams still need native language expertise for bridging, custom modules, and platform-specific features, so the promise of "one language" frequently becomes "three languages."

[One agency documented](https://www.twopicode.com/articles/this-is-why-we-stopped-using-react-native/) their experience: a single React Native project accumulated 42 NPM dependencies, of which 25 were abandoned within two years. A comparable native iOS app had 10 dependencies with only 1 outdated. The JavaScript ecosystem's rapid library churn creates ongoing maintenance overhead that native development simply does not have.

With Skip, your development language is Swift and your UI framework is SwiftUI. These are the same tools you already use for iOS development. There is no new language to learn, no NPM dependency tree to maintain, and no bridge module ecosystem to monitor for abandonment.

#### Notable departures from React Native

The most widely cited departure is [Airbnb's decision in 2018](https://medium.com/airbnb-engineering/sunsetting-react-native-1868ba28e30a) to abandon React Native and return to fully native development, after finding that bridge overhead, native module maintenance, and cross-platform inconsistencies were unsustainable with a team of approximately 100 mobile developers.

Perhaps more telling is Meta's own behavior. When Meta built Threads, their competitor to X/Twitter, in 2023, they chose native Swift/UIKit for iOS and Jetpack Compose for Android. They did not use their own React Native framework for what was arguably their highest-profile consumer app launch in years.

ThoughtWorks Technology Radar placed React Native in ["Trial" in November 2016](https://www.thoughtworks.com/radar/languages-and-frameworks/react-native) and it has not appeared on the Radar since.

---

### Skip vs. Compose Multiplatform (KMP)

Compose Multiplatform, developed by JetBrains, extends Kotlin's Jetpack Compose to iOS and other platforms. The underlying Kotlin Multiplatform (KMP) technology for sharing business logic has genuine merit, and Google endorsed KMP for shared logic at Google I/O 2024. But the UI-sharing layer, Compose Multiplatform, takes a fundamentally different approach from Skip, with trade-offs that are particularly unfavorable for iOS-first teams.

#### Compose Multiplatform does not use native iOS UI

Like Flutter, Compose Multiplatform on iOS bypasses UIKit and SwiftUI entirely. It renders all UI through [Skia with a Metal backend](https://www.jacobras.nl/2023/09/android-ios-native-flutter-compose-kmp/), drawing every component on a custom canvas. The result is an app that looks and behaves like Material Design on iOS, not like an iOS app. There is [no Cupertino widget library](https://www.kmpship.app/blog/compose-multiplatform-ios-stable-2025); developers must either accept Material Design on both platforms or build iOS-style components from scratch.

Independent size measurements tell the story clearly. [Jacob Ras's benchmarks](https://www.jacobras.nl/2023/09/android-ios-native-flutter-compose-kmp/) show that a simple Compose Multiplatform app on iOS weighs 24.8 MB, compared to 1.7 MB for the equivalent native SwiftUI app, a factor of 14.6x. This overhead comes primarily from bundling the Skia rendering library, which is built into Android but must be shipped as an additional binary on iOS. A [JetBrains GitHub issue](https://github.com/JetBrains/compose-multiplatform/issues/3632) documents developers reporting 16 MB of added overhead when integrating Compose into existing projects, with one commenter calling it "a total deal-breaker" because it exceeds the iOS App Clip size limit.

Skip produces native SwiftUI on iOS and native Jetpack Compose on Android. There is no canvas layer, no bundled rendering engine, and no size penalty. A SkipZero iOS app can ship at under 25 KB.

#### iOS support only recently reached stability

Compose Multiplatform for iOS was in Alpha through 2023 and [reached Beta in May 2024](https://blog.jetbrains.com/kotlin/2024/05/compose-multiplatform-1-6-10-ios-beta/). It was [declared Stable only in May 2025](https://blog.jetbrains.com/kotlin/2025/05/compose-multiplatform-1-8-0-released-compose-multiplatform-for-ios-is-stable-and-production-ready/) with version 1.8.0. Several features remain experimental, and [production teams report](https://www.jacobras.nl/2024/05/compose-multiplatform-ios-beta-in-2024-developer-insights/) that common components such as dialogs and flow rows were unavailable or required custom implementations. [Guarana Technologies observes](https://guarana-technologies.com/blog/kotlin-multiplatform-production) that "most teams still prefer native UI for stability" even when using KMP for shared logic.

An [academic benchmark from KTH](https://kth.diva-portal.org/smash/record.jsf?pid=diva2:1990580) found that Compose Multiplatform on iOS "exhibits frame delays during more complex animations, whereas the native implementation maintains perfect performance with no delayed frames." The study concluded that the framework is suitable for "straightforward animations and moderate performance demands" but "cannot match native Swift's capabilities in complex scenarios."

Skip has been used in production since its 1.0 release and builds on SwiftUI and Jetpack Compose, both of which have been stable and production-proven for years.

#### Kotlin is the wrong starting point for iOS teams

Compose Multiplatform requires all shared code to be written in Kotlin. For iOS teams whose expertise is in Swift, this means learning and maintaining proficiency in a second language. The interoperability story between Kotlin and Swift compounds the problem: Kotlin/Native [currently exports to Swift through an Objective-C bridge](https://medium.com/@eduardofelipi/ios-specific-integration-challenges-with-kotlin-multiplatform-75c6fa7a932e), which strips generics, collapses package namespaces, requires boxing for nullable primitives (e.g., `KotlinInt` instead of `Int?`), and forces Swift callers to specify every parameter even when Kotlin defines defaults. JetBrains is developing a [direct Swift Export](https://kotlinlang.org/docs/native-swift-export.html), but as of Kotlin 2.2 it remains experimental, does not support functional types, and cannot handle cross-language inheritance.

Skip takes the opposite approach. Your development language is Swift, your shared code is Swift, and your iOS app is pure SwiftUI. There is no language barrier and no interop bridge on the iOS side. Android interoperability with Kotlin and Java is clean and direct.

#### Garbage collection on iOS

Kotlin/Native uses a stop-the-world mark and concurrent sweep garbage collector that runs alongside Swift's ARC. [Independent measurements](https://dev.to/arsenikavalchuk/memory-management-and-garbage-collection-in-kotlin-multiplatform-xcframework-15pa) show GC epoch times reaching 10 milliseconds under stress, with XCFramework builds generating significantly more GC cycles (157 vs. 31) than shared library builds. JetBrains' own [release notes for Compose 1.7.0](https://blog.jetbrains.com/kotlin/2024/10/compose-multiplatform-1-7-0-released/) acknowledged reducing GC pause times from 1.7 ms to 0.4 ms for LazyGrid benchmarks, confirming that GC pauses were previously long enough to cause missed frames at standard refresh rates.

Skip adds no garbage collector to your iOS app. On iOS, your code runs as native Swift with ARC. There is no secondary memory management system competing for frame budget.

#### Platform risk and commercial incentives

Compose Multiplatform is primarily a JetBrains product, not a Google product. Google's endorsement of KMP at I/O 2024 [covered shared business logic but not the shared UI rendering approach](https://www.aetherius-solutions.com/blog-posts/kotlin-multiplatform-in-2026). JetBrains' ability to maintain comprehensive cross-platform tooling has come under question after [dropping KMP support from Fleet](https://news.ycombinator.com/item?id=43015387) in February 2025, and Apple's platform restrictions prevent full iOS development tooling on Windows and Linux, creating a structural dependency that JetBrains cannot resolve unilaterally.

[ThoughtWorks Technology Radar](https://www.thoughtworks.com/radar/languages-and-frameworks/kotlin-multiplatform-mobile) places Kotlin Multiplatform Mobile in the "Trial" ring, meaning it is "worth pursuing" but carries risk and has not reached the "Adopt" recommendation. [Scalability challenges on large projects](https://proandroiddev.com/kotlin-multiplatform-scalability-challenges-on-a-large-project-b3140e12da9d) have been documented, including the limitation of supporting only one KMP framework per iOS app and the impossibility of passing custom types between separately compiled frameworks.

Skip is open source and community-funded. It depends on Swift and SwiftUI on the iOS side and Kotlin and Jetpack Compose on the Android side. These are the vendor-recommended toolkits for each platform, maintained by Apple and Google respectively. Skip's role is to bridge between them, not to replace either one.

