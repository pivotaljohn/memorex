
# Application Architecture

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

