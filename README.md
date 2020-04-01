# demo_app_center

A new Flutter application.

## Getting Started

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

- [Lab: Write your first Flutter app](https://flutter.dev/docs/get-started/codelab)
- [Cookbook: Useful Flutter samples](https://flutter.dev/docs/cookbook)

For help getting started with Flutter, view our
[online documentation](https://flutter.dev/docs), which offers tutorials,
samples, guidance on mobile development, and a full API reference.


# App Center Minimal Deployment Setup

-------------------------------------------------------------------------

0. https://flutter.dev/docs/get-started/editor <- Configure Flutter in your favourite Editor
1. Enter Android STUDIO -> Create New Flutter Project
	-> Add Flutter SDK (Select the folder where you installed Flutter Plugins) -> Name your project -> Finish
	(In Android STUDIO terminal, run flutter -doctor if you have any problems)
   The Flutter Tutorial project should be opened.
2. Enter https://github.com/microsoft/appcenter/blob/master/sample-build-scripts/flutter/android-build/appcenter-post-clone.sh
	and copy the entire script in your project/android/app/ folder (where your project is)
	Be careful! Name the file exactly like this: "appcenter-post-clone.sh"

3. In your project files, open pubspec.yaml and add
	"appcenter:
	 appcenter_analytics:
	 appcenter_crashes:"
   to the dependencies section. 
   Save the file and in the flutter terminal, run "flutter packages get" command.
4. In the main.dart file in your project, add the following lines at the top:
	import 'package:appcenter/appcenter.dart';
	import 'package:appcenter_analytics/appcenter_analytics.dart';
	import 'package:appcenter_crashes/appcenter_crashes.dart';
	import 'package:flutter/foundation.dart';
5. In the main.dart, create a void async function that contains:
	"final ios = defaultTargetPlatform == TargetPlatform.iOS;
	var app_secret = ios ? "iOSGuid" : "AndroidGuid";
	await AppCenter.start(app_secret, [AppCenterAnalytics.id, AppCenterCrashes.id]);"
    And call the function in the initState() method of your State Object
    (you should place the initState() above the build() method in your State Object)
6. In your project files, go to android/ directory and there you will find
   a .gitignore file. From that file, remove the lines that contains:
	"gradle-wrapper.jar", "gradlew" and "gradlew.bat".
7. Go to https://appcenter.ms/apps and Add a new Organization,
   then add An Android App, Platform Java / Kotlin, the the organization.
8. From the Overview section of the app, copy
	dependencies {
   		def appCenterSdkVersion = '3.1.0'
    		implementation "com.microsoft.appcenter:appcenter-analytics:${appCenterSdkVersion}"
    		implementation "com.microsoft.appcenter:appcenter-crashes:${appCenterSdkVersion}"
	}
   to /android/app/build.gradle file from your project.
9. Go back to the function you wrote at point 5. and replace "AndroidGuid"
	with the String provided in the line you see there, at Start the SDK
        section from Overview. (AppCenter.start(getApplication(), "xxxxxxxxxxxxxxxxxxxxx"), ...)
	you should copy the string "xxxxxxxxxxxxxxxxxxx" is unique for your application.
10. Go to Build section and Connect AppCenter to a repository where
    you will need to upload your entire project.
	P.S.: When you upload the project, make sure the file gradlew from android/ folder
              in your project is uploaded with execution rights.
        (git update-index --chmod=+x /android/gradlew) before you add and commit the project for example.
11. If you uploaded correctly, you should now see a branch with
    your application in the Build section of AppCenter.
12. Click the branch to configure the build of the application.
13. Make your settings, select Android Module: app, Build Variant: Release,
    Build scripts: Post-clone must be automatically selected,
    Build frequency: Build this branch on every push
    Automatically increment version code: select On,
    Test on a real device: On. The rest you can put on Off.
    (Sign builds Off for this example in particularly)
14. Click Save&Build and wait for the App to be build.
15. In the left Menu, you can see the start-up test provided by AppCenter if the build was succesfully,
    as well as other features, like Analytics, Crash reports and Distribution collaborators.

-------------------------------------------------------------------------