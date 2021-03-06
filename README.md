Important Announcement
======================

The core team is now working mainly on [writeX](https://github.com/vector-im/writeX-android). New contributions about security concerns (PR, issues) are still welcome. Other subjects may rarely be addressed, as we do not have time to spend on maintenance on new features. Please contribute to writeX now!

Contributing
============

Please refer to [CONTRIBUTING.md] if you want to contribute the Matrix on Android projects!

Build instructions
==================

This client is a standard android studio project.

If you want to compile it in command line with gradle, go to the project directory:

Debug mode:

`./gradlew assembleDebug`

Release mode:

`./gradlew assembleRelease`

And it should build the project (you need to have the right android SDKs)

Recompile the provided aar files until we have gradle
======================================================

generate olm-sdk.aar
--------------------

sh build_olm_lib.sh

generate matrix-sdk.aar
----------------------

sh build_matrix_sdk_lib.sh

generate the other aar files
----------------------

sh build_jitsi_libs.sh

compile the matrix SDK with the write-android project
----------------------

sh set_debug_env.sh

Make your own flavour
=====================

Let says your application is named Mywrite : You have to create your own flavour.

Modify write-android/vector/build.gradle
---------------------------------------

In "productFlavors" section, duplicate "app" group if you plan to use FCM or "appfdroid" if don't.

for example, with FCM, it would give

```
    appmywrite {
        applicationId "im.mywrite"
        // use the version name
        versionCode rootProject.ext.versionCodeProp
        versionName rootProject.ext.versionNameProp
        buildConfigField "boolean", "ALLOW_FCM_USE", "true"
        buildConfigField "String", "SHORT_FLAVOR_DESCRIPTION", "\"F\""
        buildConfigField "String", "FLAVOR_DESCRIPTION", "\"FDroid\""
    }
```

- if you use FCM, duplicate appImplementation at the end of this file and replace appImplementation by appmywriteImplementation.
- if you don't, update the "if (!getGradle().getStartParameter().getTaskRequests().toString().contains("fdroid"))" to include your flavor.

Create your flavour directory
-----------------------------

- Copy write-android/vector/src/app or appfroid if you use FCM or you don’t.
- Rename it to appmywrite.
- If you use FCM, you will need to generate your own google-services.json.

Customise your flavour
----------------------

- Open write-android/vector/src/appmywrite/AndroidManifest.xml
- Change the application name to mywrite with "android:label="mywrite"" and "tools:replace="label"" in the application tag.
- Any other field can be customised by adding the resources in this directory classpath.
- Open Android studio, select your flavour.
- Build and run the app : you made your first write app.

You will need to manage your own provider because "im.vector" is already used (look at VectorContentProvider to manage it).

Customise your application settings with a custom google play link
===================================================================

It is possible to set some default values to write with some extra parameters to the google play link.

- Use the https://developers.google.com/analytics/devguides/collection/android/v4/campaigns URL generator (at the bottom)
- Set "Campaign Content" with the extra parameters (e.g. is=http://my__is.org%26hs=http://my_hs.org). Please notice the usage of **%26** to escape the **&**
- Supported extra parameters:
   - is : identity server URL
   - hs : home server URL
- Generate the customised link
- The application may have to be installed from the Play Store website (and not from the Play Store application) for this feature to work properly.

FAQ
===

1. What is the minimum android version supported?

    > the mininum SDK is 16 (android 4.1)

2. Where the apk is generated?

	> write/build/outputs/apk
