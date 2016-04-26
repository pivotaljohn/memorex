
# Getting Started

## Installing Android Studio

### Summary

* Pivotal machine images come with Java 6.  Uninstall that and install Oracle's Java before you install Android Studio.
* When installing Android SDKs use the SDK manager from within Android Studio (because they told you to) and *first* expand a given SDK level and install *everything* from that level.
* Get a local copy of the developer docs: `Preferences` ... `Android SDK` > `SDK Tools` > `Documentaton for Android SDK`


### 2015-04-25 — For Reel Security

#### Incident Summary

* Pivotal machine images come with Java 6.  Uninstall that and install Oracle's Java before you install Android Studio.
* When installing Android SDKs use the SDK manager from within Android Studio (because they told you to) and *first* expand a given SDK level and install *everything* from that level.

#### Play-by-Play

1. Downloaded Android Studio 2.0 (~276MB download; ~515MB on disk); copied to Applications
-  Opened Studio
-  Did not import settings
-  Got an error message: "We wanted your IDE to receive upgrades of a secure connection.  THis doesn't work on Java 6."
  * I [uninstalled the Apple Java](../java/index.md#uninstalling-the-apple-supplied-java-6)
  - Installed Oracle Java (1.8.0_91)
  - tried the install again and not more error message.
- I choose "Standard" install.  Included:
  * SDK Build tools 23.0.3
  - SDK Platform Tools 23.1
  - SDK Tools 25.1.3
  - Android Support Repository
  - Goodle Repository
  - HAXM Installer
- From the home screen, `Settings` => `Project Structure`, the JDK location was still pointing at JDK 6.

  ```bash
  /usr/libexec/java_home | pbcopy
  ```

  and set `JDK Location` in Android Studio to that value.
- Started a new project named "Canary"
  1. `(o) Phone and Tablet`
  - Android Studio downloaded 23_r03 of the SDK
  - Took defaults in creating the `MainActivity`
  - Ran the app
  - `Create New Emulator`
  - `Nexus 5X`
  - (first time I came to the `System Image`/`Recommended` view, it said, "nothing to show"; I backed up and did it again, and then four images showed up... I wonder if I had clicked the "refresh" button if that would have been the same thing).
  - `x86 Images` and selected `KitKat` (API level 19)
  - Installed x86-19_r03 to `/Users/pivotal/Library/Android/sdk`
  - Picked that image; back to the `Select Deployment Target`
  - Took a *really* long time for the enumlator to book (more than 2 minutes)
    * apparently formating the filesystem
    - after a while Android Studio got a `IOException: Broken Pipe` a couple NPEs and then stopped trying; emulator seemingly still starting up.
  - Terminated the emulator
  - Re-attempted starting app; more broken pipes.
    * I stopped the app launch.
    - *(maybe KitKat can't support this larger form-factor? ... seems an unlikely problem)*
  - Switched to `Nexus 5X` and launched emulator from the "Android Virtual Device Manager"
  - Meanwhile, attempted to launch application on the Pivotal Samsung phone and it immediately launched.
  - Deleted device from AVDM
  - `Preferences` > `Appearance & Behavior` > `System Settings` > `Android SDK` and noticed
    * the SDK Platforms list did NOT have a complete install
    - when I clicked `Show Package Details` I see that *just* the system image for KitKat was recognized by Android Studio.
  - `Launch Standalone SDK Manager`
    * No part of the KitKat emulator (Android 4.4.2) was installed according to the standalone SDK Manager.
    - read from Android website that this is intended only for "standalone" use.
  - Back in the `Preferences` ... `Android SDK`, selected `Android 4.4 (KitKat)` and clicked `Apply`
    * Notice that this is talking about API 19, rev 4 (not 3 like earlier).
  - Recreated a Virtual Device and launched it.
    * Same broken pipe dreams.
  - Back in the `Preferences` ... `Android SDK`, uninstalled everything under `Android 4.4`; clicked `Apply`
  - Back in the `Preferences` ... `Android SDK`, installed everything under `Android 4.4`; clicked `Apply`
  - Deleted Virtual Device in the AVDM
  - When to recreate new Virtual Device
    * now I see three entries for `KitKat`; two with Google APIs...
    - I picked the first one listed.
  - It finally worked!!!


> **Device Details:**
>
> Galaxy Note3; Android 4.4.2 (KitKat)

# Fundamentals

* [Fragments](http://developer.android.com/guide/components/fragments.html) were introduced in [Honeycomb](https://en.wikipedia.org/wiki/Android_Honeycomb)


## Visual Aspects

* sizing dimensions:
  * `dp` — (go-to) density-independent pixels; 1 dp = 1/160 th of the screen.
  * `sp` — (for text) dp's that are sensitive to user's scaling; all text should be in sp's.
  * `mm`, `pt`, `in` — useless as not all devices can scale these appropriately.
