# Summary

# Context

* Separating code into multiple library modules (Gradle multi-project build)
- Create additional source directories: `debug` and `release` in a library module
- Within Android Studio, set the current "Build Variant" to "debug" for all modules.

## Versions

- **Android Studio:** 2.1.1
- **Build Tools:** 24-rc4
- **Gradle:** 2.10

# Symptoms

* When deploying the app, none of the code from the `debug` source set seemed to be included in the deployed code.
  * e.g. `Caused by: java.lang.ClassNotFoundException: Didn't find class "com.reelsecurity.guardapp.greeting.StartupActivity"`
  - or worse: where there is a class in both `release` and `debug`, clearly the `release` version was being used.

# Diagnosis

* Tucked away in the [Android Gradle Plugin User Guide](https://sites.google.com/a/android.com/tools/tech-docs/new-build-system/user-guide#TOC-Library-Publication) is a note about how only the `release` build type is published, by default.  This is apparently a work in progress.
- So, the Android Studio "Build Variant" window does *not* change which variant of the library is "published" in the Gradle build.

# Prescription

1. In the root `build.gradle`, define a variable for the current build variant:

  ```groovy
  ext {
    ...
    BUILD_VARIANT = "debug"
    ...
  }
  ```
-  In each library module, reference that value as the variant to publish:

   ```groovy
   android {
       ...
       defaultPublishConfig rootProject.BUILD_VARIANT
       ...
   }
   ```
-  Whenever you change the build variant, switch BOTH in Android Studio's view *and* in this configuration variable.

## See also
