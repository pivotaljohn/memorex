# Summary

There is a bug in Espresso-Intent (v2.2.1 - ?) which causes the framework to NPE when the code-under-test fails to send an intent.

# Context

*  attempting to TDD sending an intent for the first time.
-  `applicationId` != `AndroidManifest.xml`'s `package` attribute.

## Versions

* **SDK Level (target/min):** 23 / 11
* **Android Support Library:** 23.3.0
- **Build Tools:** 23.0.3
- **Android Test Support Library:** 0.5
- **Espresso:** 2.2.2

# Symptoms


Test was failing with:

```
java.lang.NullPointerException: Attempt to invoke virtual method 'void android.support.test.espresso.intent.Intents.internalRelease()' on a null object reference
at android.support.test.espresso.intent.Intents.release(Intents.java:140)
at android.support.test.espresso.intent.rule.IntentsTestRule.afterActivityFinished(IntentsTestRule.java:68)
at android.support.test.rule.ActivityTestRule$ActivityStatement.evaluate(ActivityTestRule.java:260)
at my.package.MockServerRule$1.evaluate(MockServerRule.java:29)
at org.junit.rules.RunRules.evaluate(RunRules.java:20)
at org.junit.runners.ParentRunner.runLeaf(ParentRunner.java:325)
at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:78)
at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:57)
at org.junit.runners.ParentRunner$3.run(ParentRunner.java:290)
at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:71)
at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:288)
at org.junit.runners.ParentRunner.access$000(ParentRunner.java:58)
at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:268)
at org.junit.runners.ParentRunner.run(ParentRunner.java:363)
at org.junit.runners.Suite.runChild(Suite.java:128)
at org.junit.runners.Suite.runChild(Suite.java:27)
at org.junit.runners.ParentRunner$3.run(ParentRunner.java:290)
at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:71)
at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:288)
at org.junit.runners.ParentRunner.access$000(ParentRunner.java:58)
at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:268)
at org.junit.runners.ParentRunner.run(ParentRunner.java:363)
at org.junit.runner.JUnitCore.run(JUnitCore.java:137)
at org.junit.runner.JUnitCore.run(JUnitCore.java:115)
at android.support.test.internal.runner.TestExecutor.execute(TestExecutor.java:54)
at android.support.test.runner.AndroidJUnitRunner.onStart(AndroidJUnitRunner.java:240)
at android.app.Instrumentation$InstrumentationThread.run(Instrumentation.java:1976)
```

When app was run outside of the test:

```
05-12 06:36:07.250 10652-10652/com.reelsecurity.guardapp E/AndroidRuntime: FATAL EXCEPTION: main
  Process: com.reelsecurity.guardapp, PID: 10652
  android.content.ActivityNotFoundException: Unable to find explicit activity class {com.reelsecurity/com.reelsecurity.activity.LoginActivity}; have you declared this activity in your AndroidManifest.xml?
  at android.app.Instrumentation.checkStartActivityResult(Instrumentation.java:1777)
  at android.app.Instrumentation.execStartActivity(Instrumentation.java:1499)
  at android.app.Activity.startActivityForResult(Activity.java:3939)
  at android.support.v4.app.ActivityCompatJB.startActivityForResult(ActivityCompatJB.java:30)
  at android.support.v4.app.ActivityCompat.startActivityForResult(ActivityCompat.java:162)
  at android.support.v4.app.FragmentActivity.startActivityFromFragment(FragmentActivity.java:915)
  at android.support.v4.app.FragmentActivity$HostCallbacks.onStartActivityFromFragment(FragmentActivity.java:1010)
  at android.support.v4.app.Fragment.startActivity(Fragment.java:921)
  at android.support.v4.app.Fragment.startActivity(Fragment.java:910)
  at com.reelsecurity.guardapp.greeting.GreetingFragment$1.onClick(GreetingFragment.java:34)
  at android.view.View.performClick(View.java:5181)
  at android.view.View$PerformClick.run(View.java:20887)
  at android.os.Handler.handleCallback(Handler.java:739)
  at android.os.Handler.dispatchMessage(Handler.java:95)
  at android.os.Looper.loop(Looper.java:145)
  at android.app.ActivityThread.main(ActivityThread.java:5942)
  at java.lang.reflect.Method.invoke(Native Method)
  at java.lang.reflect.Method.invoke(Method.java:372)
  at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:1400)
  at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:1195)
```

# Diagnosis

-  There's a bug in the Espresso-Intent library ([187249](https://code.google.com/p/android/issues/detail?id=187249)) that manifests the NPE whenever an attempt to send an intent fails in the code-under-test.
-  We were failing to send the right intent in our code-under-test because we were specifying the value for `package` (`AndroidManifest.xml`) as the target package for the intent instead of the value specified in the `build.gradle`'s `android` section, `applicationId`.

# Prescription

*  Wait for the bug to be fixed; in the meantime, know that this error message means that the intent wasn't successfully sent.
*  When sending explicit intents, use the `applicationId` from your `build.gradle` as the "package name", not the `package` attribute from the `AndroidManifest.xml`.
-  When sending implicit intents via an Action, be sure to include the `DEFAULT_CATEGORY` to that activity's `<intent-filter>`, otherwise the intent will not match.
-  When mocking intents, always include an `.respondWith()`; otherwise your intent is not actually stubbed.

## See also

* The different between `applicationId` and `packagename`: [Android Tech Docs](https://sites.google.com/a/android.com/tools/tech-docs/new-build-system/applicationid-vs-packagename).
* Google Code bug report [187249](https://code.google.com/p/android/issues/detail?id=187249).

