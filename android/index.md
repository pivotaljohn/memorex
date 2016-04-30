
# Getting Started

## Installing Android Studio

### Summary

* Pivotal machine images come with Java 6.  Uninstall that and install Oracle's Java before you install Android Studio.
* When installing Android SDKs use the SDK manager from within Android Studio (because they told you to) and *first* expand a given SDK level and install *everything* from that level.
* Get a local copy of the developer docs: `Preferences` ... `Android SDK` > `SDK Tools` > `Documentaton for Android SDK`

Install Sessions:

* [Reel Security (circa April 2016)](./install-notes--2016-04-25.md)


# Fundamentals

## Application Architecture

### The Things

* [Activities](https://developer.android.com/guide/components/activities.html) encapsulate a single screen of interaction with the user.
  * which are composed from UI chunks or [Fragments](http://developer.android.com/guide/components/fragments.html).
- [Intents](https://developer.android.com/guide/components/intents-filters.html) are messages sent to Activites, Services, and Broadcast Receivers.  Intents are messages sent between "application components".
  * the payload of an Intent is an "Action", "Category" and "Extras".
  * invoking Activities can request a response (which arrives in the form of an Intent)
- [Services](https://developer.android.com/guide/components/services.html) encapsulate "backend" behavior that requires no UI.
  * should be used for any work that is "long-running" (see [Process and Thread Model](#process-and-thread-model), below.
- [Broadcast Receivers](https://developer.android.com/reference/android/content/BroadcastReceiver.html) responses to system-wide events (via an Intent).
- [Content Providers](https://developer.android.com/guide/topics/providers/content-providers.html) encapsulate a data store meant to be shared with other applications (e.g. Contacts).
- [App Widgets](https://developer.android.com/guide/topics/appwidgets/index.html) are mini applications that are meant to be used within another application (e.g. mini music player on the Home screen).

### Process and Thread Model

#### Scheduling / Lifecycle

*  Each application begins as a process and a single thread.
-  Processes are kept alive as long as possible, but are terminated, least important to most.
-  The priorities are this (most to least):
   1. activities, services, and broadcast receivers that are actively doing work.
   -  activities that are paused but still visible (or services bound to such activities)
   -  background services (i.e. launched with `startService()`)
   -  backgrounded activities (i.e. that have been `onStop()`'ed.
-  Practical implication: a process doing the same work through a Service will be higher priority than a process using an async task. 

#### Multi-Threading

* Only do work within the UI thread when it costs less to do than spawning a new thread.
- Use [`AsyncTask`](https://developer.android.com/reference/android/os/AsyncTask.html) to background work efficiently.
  * fully featured.  Allows you to report progress and be cancelled.
- Work invoked through bound services occur not in the UI thread but from an OS-managed thread pool.
- ContentProviders process requests in within its own process (and therefore its own thread pool)


## General Attitudes

* Android is an object-oriented system, designed to support lots of little things signaling to each other.
* Design applications to operate as cooperating cells.
* Individual components (Activities, Fragments, Services, etc.) must be resilient — design to survive sudden stops and restarts (e.g. when a user rotates the device).
* [AndroidManifest.xml is a public API of your "app"](http://android-developers.blogspot.com/2011/06/things-that-cannot-change.html)
* Activities and Broadcast Receivers should have very quick transaction times; anything longer should become a Service.

## Implementation Guidelines

### The Activity Lifecycle

*  `onCreate()` — allocate resources, start fetches,
-  `onStart()` — 
-  `onRestoreInstanceState()` — restore UI and local app state 
-  `onResume()` — 
-  `onPause()` — store data that should be persisted across application launches or that might be used by an Activity taking over.
-  `onSaveInstanceState()` — save local app state *(not called if app is definitely quitting)* 
-  `onStop()`
   * cancel any background tasks (e.g. `AsyncTask` instances) 
-  `onDestroy()`


Out-of-The Box:
* Most `View`s can save their own state (and will do so via the default implementation of `Activity.onSaveInstanceState()`).
  
  > **Tip:** only widgets with an `android:id` will be saved.
  

## Visual Aspects

* sizing dimensions:
  * `dp` — (go-to) density-independent pixels; 1 dp = 1/160 th of the screen.
  * `sp` — (for text) dp's that are sensitive to user's scaling; all text should be in sp's.
  * `mm`, `pt`, `in` — useless as not all devices can scale these appropriately.

# Testing

## Summary

* Use Espresso over Robotium
  * E. synchronizes the Instrumentation thread with the UI thread while R. uses sleeps.
  - R. exposes objects that should not be modified off of the UI thread
  - E. is used internally by Google
  - sources: 
    * [StackOverflow: Google Espresso or Robotium](http://stackoverflow.com/questions/20046021/google-espresso-or-robotium)
    * [Greenhouse CI: Making UI testing on Android easy](http://blog.greenhouseci.com/greenhouse/update/robotium-and-espresso/)

