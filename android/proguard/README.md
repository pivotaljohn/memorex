

# Raw Notes

* By default, ProGuard removes code that is not explicitly referenced by seeds, transitively.
* You can keep classes by:
  * including a line like `-keep public class MyClass` in the `proguard-rules.pro` file.
  * adding the `@Keep` annotation
    * defined in the [Annotations Support Library](https://developer.android.com/topic/libraries/support-library/features.html#annotations).


# Resources

* (ProGuard Troubleshooting](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/index.html#manual/troubleshooting.html)
