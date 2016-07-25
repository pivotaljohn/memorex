
# Application Architecture

## Chunk Application Into Modules

* for each Activity/Content Provider/Sync Adapter/(android component), create a separate [Android Library](https://sites.google.com/a/android.com/tools/tech-docs/new-build-system/user-guide#TOC-Library-projects) module.
  * see also: [build](../build) for details of how to make the app module dependent on the libraries.

## Dependency Injection with Dagger2

* [(https://github.com/google/dagger/tree/master/examples/android-simple/src/main/java/com/example/dagger/simple)

## Resources:

* [Martin Fowler on GUI Architectures](http://www.martinfowler.com/eaaDev/uiArchs.html)
* [a reading of the Android Testing Codelab code](./reading--android-codelab.md).
  * Illustrates MVP in practice
  - also a lot of other "best practice" potentials.

----

* Create Application Component Modules and Libraries using Android Studio.

Minimum `build.gradle` for a raw Library:

```groovy
apply plugin: 'java'

targetCompatibility = '1.7'
sourceCompatibility = '1.7'
```

