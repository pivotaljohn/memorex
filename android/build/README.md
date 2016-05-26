

# Group Dependencies by Need

... in `build.gradle`

* you can always get a listing of the classpath using `./gradlew dependencies` but nothing else will tell you what set of dependencies are present for a given need.

# Resolving unwanted transitive dependencies

Because the app APK and test APK share a classpath, they have to agree on library versions (and it's just good practice anyway).

(see also [Android Gradle Plugin Technical Notes: Resolving Conflicts Between Main and Test APKs](https://sites.google.com/a/android.com/tools/tech-docs/new-build-system/user-guide#TOC-Resolving-conflicts-between-main-and-test-APK)).

**For example...**

Message:

You'll get this from Gradle and/or Android Studio...

```
Error:Conflict with dependency 'junit:junit'.
  Resolved versions for app (4.11) and test app (4.12) differ.
  See http://g.co/androidstudio/app-test-app-conflict for details.
 ```

Action:

1.  In the root `build.gradle`, fail fast on version conflicts

   ```groovy
   configurations.all {
      // Tolerating version conflicts can be risky.  Instead, fail fast and resolve
      // conflicts using "force", below.
      failOnVersionConflict()
      ...
   ``` 
-  Check dependencies:

   ```bash
   ./gradlew app:dependencies

   ```
-  When the transitive dependency is added to an "earlier" configuration (e.g. `[buildType]compile`), you'll likely exclude the conflicting transitive dependency and then re-add the proper version.
-  Back in the root `build.gradle`, `force` the resolution:

   ```groovy
   configurations.all {
      // Tolerating version conflicts can be risky.  Instead, fail fast and resolve
      // conflicts using "force", below.
      failOnVersionConflict()
      
      force "junit:junit:$rootProject.JUNIT_VER"
      force "com.android.support:support-v4:${rootProject.ANDROID_SUPPORT_LIBRARY_VER}"
      force "com.android.support:support-annotations:$rootProject.ANDROID_SUPPORT_LIBRARY_VER"
   }
    ```
