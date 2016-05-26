
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

1. List dependencies,

   ```bash
   ./gradlew app:dependencies | less
   ```
-  When the transitive dependency is added to an "earlier" configuration (e.g. `[buildType]compile`), you'll likely exclude the conflicting transitive dependency and then re-add the proper version.

   ```groovy
    // Mock Web Server
    debugCompile("com.squareup.okhttp3:mockwebserver:3.2.0"){
        exclude group: 'junit', module: 'junit' // likely mismatches version of junit we're using.
    }
    debugCompile "junit:junit:$rootProject.JUNIT_VER"
    ```
  * it is possible for the contributed version to be earlier *or* later than the desired one, by `exclude`'ing and then explicitly fingering the specific version we're using is the "safest" way to manage the dependency.
