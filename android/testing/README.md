
# Overall Approach

1. Isolate as much custom logic into POJOs that define contracts between it, UI components and other resources (e.g. repositories, Services, Content Providers), test that logic mocking the "outside world" in JUnit tests that run on the JVM.
-  For Android resources that are sufficiently painful to mock, write [unit tests that run on the ART/Dalvik](https://github.com/googlesamples/android-testing/tree/master/unit/BasicUnitAndroidTest) using the live resources.
-  To ensure a baseline confidence that the app functions correctly, write a [Journey Test](http://martinfowler.com/bliki/UserJourneyTest.html) using Espresso.
-  ...

# Responsibilities/Contracts

* Activity
- Fragment
- Service


# Android Testing Support Library (ATSL)

See [Android Testing Support Library (ATSL) Documentation](https://google.github.io/android-testing-support-library/docs/index.html).


## Espresso vs. Robotium

Espresso is superior to Robotium:
* E. synchronizes the Instrumentation thread with the UI thread while R. uses sleeps.
  * ensures that the Main/UI thread is idle
  - ensures that the AsyncTask thread pool is idle
  - provides a counting semaphore ([IdlingResource](https://developer.android.com/reference/android/support/test/espresso/IdlingResource.html)) to allow applications to be more test-friendly by indicating when an operation is in-flight. (see also [Chiu-Ki Chan's article on Custom Idling Resources](http://blog.sqisland.com/2015/04/espresso-custom-idling-resource.html)).
- R. exposes objects that should not be modified off of the UI thread
- sources:
    * [StackOverflow: Google Espresso or Robotium](http://stackoverflow.com/questions/20046021/google-espresso-or-robotium)
    * [Greenhouse CI: Making UI testing on Android easy](http://blog.greenhouseci.com/greenhouse/update/robotium-and-espresso/)


# Raw Notes

* Instrumentation Tests allow you to "shard" your test suite — easily splitting your tests into chunks and running just the one chunk.  This allows you to do parallel testing.
* `AndroidJUnitRunner` replaces `InstrumentationTestRunner` (JUnit 3 only)
  ![IntrumentationTestRunner](https://developer.android.com/images/testing/test_framework.png)

> **Tip:**
 You need to turn off animations on your test device (or emulator), otherwise Espresso may not work as expected and your tests may fail.  For example, the tests may spin for a long time and fail complaining that the "Activity was not launched."  Turn off animations from Settings by opening Developer Options and turning all the following options under “Drawing” off:
>
* Window animation scale
- Transition animation scale
- Animator duration scale
>
*source: [Google Codelabs — Android Testing](https://codelabs.developers.google.com/codelabs/android-testing/index.html?index=..%2F..%2Findex#6)*

# Resources

* [Google Codelabs](https://codelabs.developers.google.com/?cat=Android)
* [Getting Started With Testing](https://developer.android.com/training/testing/start/index.html)
* [Android Testing Tools](https://developer.android.com/tools/testing/index.html)
  * [Android Testing Support Library](https://developer.android.com/tools/testing-support-library/index.html)
* [Test Sizes](http://googletesting.blogspot.com/2010/12/test-sizes.html)


----
# The Big Day of Testing: May 20, 2016

* Clean-up!  We weren't thinking about what state was being left behind by each test (in our case, the file that was storing the "last known stage".  Also, there was the Mock Web Server in a Service that was running between each test.
* If your starting Activity emits intents that you want to stub (e.g. starting an Activity that's in another module on which yours does not depend), you MUST use an `ActivityTestRule` not an `IntentTestRule`.
  * You think that since you're dealing with intents, you ought to use `IntentsTestRule`...
  * If you want to stub an intent (i.e. `intending(..)`) before the activity launches, then you must configure the `IntentTestRule` to **not** start the activity.
  * Before you do the stubbing, you must call `Intents.init()` first.
  * `IntentTestRule#launchActivity()` *also* calls `Intents.init()` "for you"; you can't `init()` twice and you'll get an exception.
  * So:
    1. use `ActivityTestRule` instead, configured to not launch the Activity
    2. call `Intents.init()` (prolly in your `@Before`)
    3. do your stubbing appropriate to your test within your `@Test`
    4. call `mActivityTestRule.launchActivity(new Intent())` when you're ready to start the Activity.
       * note that `ActivityTestRule` does not call `Intents.init()` and you won't get that exception.
    5. call `Intents.release()` (also likely in your `@After`) to cleanup the Intents business.

