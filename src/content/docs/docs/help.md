---
title: Getting Help
permalink: /docs/help/
---

The best way to get help and connect with fellow Skip users is to join the community [Slack](/slack/), where you can find real-time support and the community expertise to help you succeed with your project. You can also use the Skip [discussion forums](http://forums.skip.dev) to send feedback, ask questions, or search for solutions to issues. For bug reports, use the [issue tracker](https://source.skip.dev/skip/issues).

Please include the output of the `skip checkup` command in any communication related to technical issues.

## Troubleshooting Common Issues {#troubleshooting}

Skip's architecture relies on recent advances in the plugin system used by Xcode 15 and Swift Package Manager 5.9. When unexpected issues arise, often the best first step is to clean your Xcode build (`Product` → `Clean Build Folder`) and reset packages (`File` → `Packages` → `Reset Package Caches`). Restarting Xcode is sometimes warranted, and trashing the local `DerivedData` folder as well as your app directory's `.build` folder might even be needed. 

Specific known error conditions are listed below. Search the [documentation](/docs), [issues](https://source.skip.dev/skip/issues), and [discussions](http://forums.skip.dev) for more information and to report problems.

### `skip doctor` / `skip checkup` errors

#### "Swiftly version: error executing swiftly"

[`swiftly`](https://github.com/swiftlang/swiftly) is a tool to install and manage Swift toolchains. Homebrew installs `swiftly` when you install `skip`.

If you've never used `swiftly` before, you'll need to initialize it, by running `swiftly init`. Then, `swiftly --version` should run and provide you with a version number. Once `swiftly --version` works, `skip doctor` will no longer have an error executing `swiftly`.

#### "Xcode version: error executing xcodebuild"

When you view the log, you'll probably see an error like this:

```plaintext
xcode-select: error: tool 'xcodebuild' requires Xcode, but active developer
directory '/Library/Developer/CommandLineTools' is a command line tools instance
```

To resolve this error, run this:

```console
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
```

You'll need to enter your macOS password to update it. (If you prefer, you can open Xcode, navigate to Settings --> Locations, and update "Command Line Tools", changing it to your current version of Xcode.)

#### "xcodebuild: error: Unable to find a destination matching the provided destination specifier:"

Check the `skip checkup` log, and you might find an error like this:

```plaintext
xcodebuild: error: Unable to find a destination matching the provided destination specifier:
    { generic:1, platform:iOS }
Available destinations for the "HelloSkip App" scheme:
    { platform:macOS, arch:arm64, id:01f95e76beb7a42fa85328bb400a00e1ed54f993, name:My Mac }
    { platform:macOS, arch:x86_64, id:01f95e76beb7a42fa85328bb400a00e1ed54f993, name:My Mac }
    { platform:macOS, name:Any Mac }
Ineligible destinations for the "HelloSkip App" scheme:
    { platform:iOS, id:dvtdevice-DVTiPhonePlaceholder-iphoneos:placeholder, name:Any iOS Device, error:iOS 26.2 is not installed. Please download and install the platform from Xcode > Settings > Components. }
```

In this case, you'll need to install the iOS SDK in Xcode > Settings > Components.

#### "SDK location not found. Define a valid SDK location with an ANDROID_HOME environment variable"

You'll typically see this error in the log when the "Test Kotlin" step fails during `skip checkup`. The full error typically looks like this:

```plaintext
execGradle: /opt/homebrew/bin/gradle testDebug --project-dir /private/var/folders/1l/jbmc3w7947x9ljkj_260bmrc0000gn/T/902632B6-1BB4-47D5-98A1-5E55CF286A78/.build/checkouts/skip/Skip/build/902632b6-1bb4-47d5-98a1-5e55cf286a78/HelloSkipTests/destination/skipstone --warning-mode all -PbuildDir=.build/HelloSkip --rerun-tasks --console=plain
FAILURE: Build failed with an exception.
* What went wrong:
Could not determine the dependencies of task ':HelloSkip:testDebugUnitTest'.
> SDK location not found. Define a valid SDK location with an ANDROID_HOME environment variable or by setting the sdk.dir path in your project's local properties file at '/private/var/folders/1l/jbmc3w7947x9ljkj_260bmrc0000gn/T/902632B6-1BB4-47D5-98A1-5E55CF286A78/.build/plugins/outputs/902632b6-1bb4-47d5-98a1-5e55cf286a78/HelloSkipTests/destination/skipstone/local.properties'.
```

You'll need to install Android Studio and run it, which will download the Android SDK and install it in `~/Library/Android/sdk`. (If your Android SDK is installed in another directory, specify it with the `$ANDROID_HOME` environment variable.)
