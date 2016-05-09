# Questions

* Q01: are variants automatically generated for you?  flavor vs. buildType?  What are these for?
* Q02: where do I find all of the switches and dials for the `com.android.application` plugin?
* Q03: Power Mockito is being declared as a dependency; where is it being used?
* Q04: why in `apps/build.gradle` do they use `resolutionStrategy.force` for the `support-annotations` library?  why not just included the dependency in both `test` and `compile`?!?
* Q05: How do I determine how many methods are in my final app? (re: the 64k DEX method limit)
* Q06: There a reference to bug [192497](https://code.google.com/p/android/issues/detail?id=192497) but when I read it, that bug is closed: "not reproduceable"; what's this bug about?
* Q07: What are all the special directories that can be in a source set root for an android library/app?  In this app, they are using `assets` to store an image.  Why would I do that over say have that be a resource?
* Q08: `AndroidManifest.xml`: I see that all the activities are registered; is this necessary or somehow good form?
* Q09: `AndroidManifest.xml`: what does `allowBackup=false` do?
* Q10: `AndroidManifest.xml`: what does `supportsRtl` do?

# Highlighted Techniques

## Model-View-Presenter pattern

This is one of the main points of this code example: separating as much of the core logic from the boilerplate GUI plumbing.

Nuts & bolts, this becomes a 4-tuple of classes:

* a `...Contract` — an interface that defines the responsibilities of the `View` and the `Presenter` with respect to the named screen.  Within that two inner interfaces:
  * `UserActionListener`<sup>1</sup> — defines the various actions a user can take.
  - `View` — what the user-facing components must be able to do.
    - including `setUserActionListener()` because the "View" is responsible for wiring GUI widgets with the corresponding `UserActionListener` actions.
* a `...Presenter` the fulfills the responsibilities of the `UserActionListener`.

-

<sup>1</sup> hmmm... can we come up with a more terse and equally intention-revealing name?  "View" is so short and sweet.  Perhaps: "Actor"?  or "Actuator"?

## Wrapping *any* Resource

Even seemingly simple resources (e.g. [ImageFileImpl](https://github.com/googlecodelabs/android-testing/blob/305e806d819d068e8f960263865755ead8676fa8/app/src/main/java/com/example/android/testing/notes/util/ImageFileImpl.java)) are extracted with an interface in front to facilitate injecting a testing/mock implementation.


## Instrumenting for Espresso

* Note that Espresso does this automatically for known resources (e.g. AsyncTask thread pool).
- It requires signals from actual code when a long-running process has launched.  This means that something like a [EspressoIdlingResource](https://github.com/googlecodelabs/android-testing/blob/305e806d819d068e8f960263865755ead8676fa8/app/src/main/java/com/example/android/testing/notes/util/EspressoIdlingResource.java) is used during network or filesystem calls.  This will significantly increase UI testing stability as the test framework can reliably wait for true idle.
- In fact, where possible, (e.g. [using Retrofit](http://michaelevans.org/blog/2015/08/03/using-espresso-for-easy-ui-testing/)), configure long-running task code to run on the AsyncTask pool and you'll get that synchronization for free!
- When not possible, then you can still participate by registering an [IdlingResource](https://developer.android.com/reference/android/support/test/espresso/IdlingResource.html).
  * When [AddNoteFragment#showImagePreview()](https://github.com/googlecodelabs/android-testing/blob/305e806d819d068e8f960263865755ead8676fa8/app/src/main/java/com/example/android/testing/notes/addnote/AddNoteFragment.java#L163) loads an image and then signals from within the [`onResourceReady()`](https://github.com/googlecodelabs/android-testing/blob/305e806d819d068e8f960263865755ead8676fa8/app/src/main/java/com/example/android/testing/notes/addnote/AddNoteFragment.java#L175) callback when done.
  - and in general, searching for [`EspressoIdlingResource.increment();`](https://github.com/googlecodelabs/android-testing/search?utf8=%E2%9C%93&q=EspressoIdlingResource.increment%28%29%3B)

## Light-Weight Dependency Injection

* in `app/build.gradle` defined two `productFlavors`: `mock` and `prod`
  - `mock` must be a different app so: `applicationIdSuffix = ".mock"`

## Versions at the Root Gradle Project

Defined all dependencies at root `build.gradle`
* separated into SDK & Tools and application dependencies
- makes it really easy both to read which versions are in play *and* to change them.

