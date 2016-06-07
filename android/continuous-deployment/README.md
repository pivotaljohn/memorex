

# Google Play

*  There are three "channels": alpha, beta, and production.
-  


> **Tip:**
>
> Any changes to your listing in the Google Play Console can take hours to process.  Until it is, you may not be able to make other changes to the listing.

# Managing Android Tooling for CI

* https://github.com/makinacorpus/android-archetypes/wiki/Getting-started%3A-Building-Android-apps-with-Jenkins-CI
* the `android` command-line tool obtains metadata from a set of XML files.

    ```bash
    android list sdk
    Refresh Sources:
      Fetching https://dl.google.com/android/repository/addons_list-2.xml
      Validate XML
      Parse XML
      Fetched Add-ons List successfully
      Refresh Sources
      Fetching URL: https://dl.google.com/android/repository/repository-11.xml
      Validate XML: https://dl.google.com/android/repository/repository-11.xml
      Parse XML:    https://dl.google.com/android/repository/repository-11.xml
      Fetching URL: https://dl.google.com/android/repository/addon.xml
      Validate XML: https://dl.google.com/android/repository/addon.xml
      Parse XML:    https://dl.google.com/android/repository/addon.xml
      Fetching URL: https://dl.google.com/android/repository/glass/addon.xml
      Validate XML: https://dl.google.com/android/repository/glass/addon.xml
      Parse XML:    https://dl.google.com/android/repository/glass/addon.xml
      Fetching URL: https://dl.google.com/android/repository/extras/intel/addon.xml
      Validate XML: https://dl.google.com/android/repository/extras/intel/addon.xml
      Parse XML:    https://dl.google.com/android/repository/extras/intel/addon.xml
      Fetching URL: https://dl.google.com/android/repository/sys-img/android/sys-img.xml
      Validate XML: https://dl.google.com/android/repository/sys-img/android/sys-img.xml
      Parse XML:    https://dl.google.com/android/repository/sys-img/android/sys-img.xml
      Fetching URL: https://dl.google.com/android/repository/sys-img/android-wear/sys-img.xml
      Validate XML: https://dl.google.com/android/repository/sys-img/android-wear/sys-img.xml
      Parse XML:    https://dl.google.com/android/repository/sys-img/android-wear/sys-img.xml
      Fetching URL: https://dl.google.com/android/repository/sys-img/android-tv/sys-img.xml
      Validate XML: https://dl.google.com/android/repository/sys-img/android-tv/sys-img.xml
      Parse XML:    https://dl.google.com/android/repository/sys-img/android-tv/sys-img.xml
      Fetching URL: https://dl.google.com/android/repository/sys-img/google_apis/sys-img.xml
      Validate XML: https://dl.google.com/android/repository/sys-img/google_apis/sys-img.xml
      Parse XML:    https://dl.google.com/android/repository/sys-img/google_apis/sys-img.xml
    Packages available for installation or update: 27
       1- SDK Platform Android N Preview, revision 3
       ...
    ```
* To determine the value for `filter`, curl the XMLs, grep by the display name

    ```
    $ npm install -g xml2json-command
    $ brew install jq
    $ curl https://dl.google.com/android/repository/repository-11.xml | xml2json | jq
    ```
