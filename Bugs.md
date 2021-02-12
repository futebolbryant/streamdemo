## Stream Flutter SDK Bug Tracking and Crucial Feature Requests
stream_chat_flutter: ^1.0.2-beta

#### Notes
- See two sections, Bugs and Feature Requests
- The most concerning bug is #3 as it renders the app completely unusable

#### My Test Devices
- Android Physical: moto g6, android version 9, build number PDS29.118-15-11-14
- iPhone Physical: iPhone 11 Pro, iOS 14.4 (18D52)
- Android emulator: Pixel 4 API 30
- *Note: I always ran the app in debug mode*

# Bugs

### Bug 1

Description: Overflow errors in channel list 

Repro steps: 
- Start app
- Observe overflow errors in console
- Expected: No overflow errors

Log: (there are multiple of the same message)
```
======== Exception caught by rendering library =====================================================
A RenderFlex overflowed by 8.0 pixels on the right.
The relevant error-causing widget was:
Row file:///Users/myname/flutter/.pub-cache/hosted/pub.dartlang.org/stream_chat_flutter-1.0.2-beta/lib/src/channel_list_view.dart:404:21
====================================================================================================
```

### Bug 2

Description: Android App crashes on first load due to permissions check

Repro:
- Use Android Physical Device
- Ensure demo app not already installed
- Start app, go to channel page, tap attachment icon
- App opens permissions dialog
- App crashes (see log), dialog remains
- Tap 'allow' in dialog
- Restart app
- You can now upload attachments
- Expected: App does not crash when tapping attachment icon for the first time

```
W/Activity( 2066): Can request only one set of permissions at a time
E/AndroidRuntime( 2066): FATAL EXCEPTION: pool-4-thread-1
E/AndroidRuntime( 2066): Process: com.example.streamdemo, PID: 2066
E/AndroidRuntime( 2066): java.lang.SecurityException: Permission Denial: reading com.android.providers.media.MediaProvider uri content://media/external/file from pid=2066, uid=10208 requires android.permission.READ_EXTERNAL_STORAGE, or grantUriPermission()
E/AndroidRuntime( 2066): 	at android.os.Parcel.createException(Parcel.java:1950)
E/AndroidRuntime( 2066): 	at android.os.Parcel.readException(Parcel.java:1918)
E/AndroidRuntime( 2066): 	at android.database.DatabaseUtils.readExceptionFromParcel(DatabaseUtils.java:183)
E/AndroidRuntime( 2066): 	at android.database.DatabaseUtils.readExceptionFromParcel(DatabaseUtils.java:135)
E/AndroidRuntime( 2066): 	at android.content.ContentProviderProxy.query(ContentProviderNative.java:418)
E/AndroidRuntime( 2066): 	at android.content.ContentResolver.query(ContentResolver.java:802)
E/AndroidRuntime( 2066): 	at android.content.ContentResolver.query(ContentResolver.java:752)
E/AndroidRuntime( 2066): 	at android.content.ContentResolver.query(ContentResolver.java:710)
E/AndroidRuntime( 2066): 	at top.kikt.imagescanner.core.utils.DBUtils.getGalleryList(DBUtils.kt:56)
E/AndroidRuntime( 2066): 	at top.kikt.imagescanner.core.PhotoManager.getGalleryList(PhotoManager.kt:51)
E/AndroidRuntime( 2066): 	at top.kikt.imagescanner.core.PhotoManagerPlugin$onHandlePermissionResult$1.invoke(PhotoManagerPlugin.kt:213)
E/AndroidRuntime( 2066): 	at top.kikt.imagescanner.core.PhotoManagerPlugin$onHandlePermissionResult$1.invoke(PhotoManagerPlugin.kt:29)
E/AndroidRuntime( 2066): 	at top.kikt.imagescanner.core.PhotoManagerPlugin$sam$java_lang_Runnable$0.run(Unknown Source:2)
E/AndroidRuntime( 2066): 	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1167)
E/AndroidRuntime( 2066): 	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:641)
E/AndroidRuntime( 2066): 	at java.lang.Thread.run(Thread.java:798)
I/Process ( 2066): Sending signal. PID: 2066 SIG: 9
```

### Bug 3

Description: Memory errors with images, app crashes / becomes unresponsive

Repro:
- Use Android Physical Device
- Start app, go to channel page, select any 2 images taken with phone camera (mine were 3MB+) 
- Upload these two images ~6 times, until memory errors start appearing in console (see below)
- Scroll up and down the channel page
- Memory errors should be piling up by the thousand
- Screen goes white, app is unresponsive
- Close app, restart, go back to that channel and the issue remains.  The channel is unresponsive and locks the phone up.
- Expected: No memory errors

```
W/Adreno-GSL( 2511): <sharedmem_gpuobj_alloc:2461>: sharedmem_gpumem_alloc: mmap failed errno 12 Out of memory
E/Adreno-GSL( 2511): <gsl_memory_alloc_pure:2236>: GSL MEM ERROR: kgsl_sharedmem_alloc ioctl failed.
W/Adreno-GSL( 2511): <sharedmem_gpuobj_alloc:2461>: sharedmem_gpumem_alloc: mmap failed errno 12 Out of memory
E/Adreno-GSL( 2511): <gsl_memory_alloc_pure:2236>: GSL MEM ERROR: kgsl_sharedmem_alloc ioctl failed.
```

*Notes*

Seems to have to do with the cached_network_image library.  

### Bug 4
Description: Overflow error

Repro:
- Use any device
- Start app, go to channel page, tap message input
- Android on open/close attachments menu
- Tap into message input, then tap attachments icon
- Tap back into the message input

```

======== Exception caught by rendering library =====================================================
The following assertion was thrown during layout:
A RenderFlex overflowed by 74 pixels on the bottom.

The relevant error-causing widget was: 
  Column file:///Users/johnbryant/Documents/Source/github/streamdemo/lib/main.dart:122:13
The overflowing RenderFlex has an orientation of Axis.vertical.
The edge of the RenderFlex that is overflowing has been marked in the rendering with a yellow and black striped pattern. This is usually caused by the contents being too big for the RenderFlex.

Consider applying a flex factor (e.g. using an Expanded widget) to force the children of the RenderFlex to fit within the available space instead of being sized to their natural size.
This is considered an error condition because it indicates that there is content that cannot be seen. If the content is legitimately bigger than the available space, consider clipping it with a ClipRect widget before putting it in the flex, or using a scrollable container rather than a Flex, like a ListView.

The specific RenderFlex in question is: RenderFlex#0df88 relayoutBoundary=up1 OVERFLOWING
...  needs compositing
...  parentData: offset=Offset(0.0, 80.0); id=_ScaffoldSlot.body (can use size)
...  constraints: BoxConstraints(0.0<=w<=360.0, 0.0<=h<=343.0)
...  size: Size(360.0, 343.0)
...  direction: vertical
...  mainAxisAlignment: start
...  mainAxisSize: max
...  crossAxisAlignment: center
...  verticalDirection: down
◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤
====================================================================================================

======== Exception caught by rendering library =====================================================
The following assertion was thrown during layout:
A RenderFlex overflowed by 29 pixels on the bottom.

The relevant error-causing widget was: 
  MessageInput file:///Users/johnbryant/Documents/Source/github/streamdemo/lib/main.dart:133:11
The overflowing RenderFlex has an orientation of Axis.vertical.
The edge of the RenderFlex that is overflowing has been marked in the rendering with a yellow and black striped pattern. This is usually caused by the contents being too big for the RenderFlex.

Consider applying a flex factor (e.g. using an Expanded widget) to force the children of the RenderFlex to fit within the available space instead of being sized to their natural size.
This is considered an error condition because it indicates that there is content that cannot be seen. If the content is legitimately bigger than the available space, consider clipping it with a ClipRect widget before putting it in the flex, or using a scrollable container rather than a Flex, like a ListView.

The specific RenderFlex in question is: RenderFlex#ff95b relayoutBoundary=up11 OVERFLOWING
...  parentData: <none> (can use size)
...  constraints: BoxConstraints(0.0<=w<=360.0, h=39.2)
...  size: Size(360.0, 39.2)
...  direction: vertical
...  mainAxisAlignment: start
...  mainAxisSize: max
...  crossAxisAlignment: center
...  verticalDirection: down
◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤
====================================================================================================
```

*Notes*

- Happens almost every time for me, but a few times it did not happen, not sure why
- See screenshots/OpenAttachments.png (Note that this is a screenshot from a screen recording)

### Bug 5
Description: Overflow error on long system messages

Repro:
- Use any device
- Add a long system message to channel 
- Observed: System  message overflows and text does not wrap
- Expected: text should wrap

*Notes*

See screenshots/SystemMessageOverflow.png

### Bug 6
Description: Searched giphy text does not show for **my** message after sending 

Repro:
- Use any device
- Search for, choose, send giphy
- Observed: Giphy shows, but you can't see what you searched for
- Expected: You can see the giphy search text

*Notes*

I find this to be a bug because you can see this for other channel members but not yourself

# Feature Requests

Note: These are not bugs, but are crucial features to have to use this SDK IMO

### Feature Request 1

Description: Keyboard doesn't close after searching for giphy

Repro:
- Use any device
- Start app, go to channel page
- Tap instant commands, search for giphy 'test' 
- Tap search icon
- Keyboard stays open, you sometimes can't see the giphy search result
- Expected: keyboard closes after tapping search, you shouldn't have to close it manually

### Feature Request 2

Description: Scrolling up in message history or tapping within message list should close keyboard

Repro:
- Go to channel page, open message input
- Scroll upwards through history
- Tap anywhere message list
- Observed: Keyboard stays

This is especially annoying on iPhone as there is no way to close the keyboard other than swiping down on the message input which I found unintuitive

### Feature Request 3

Description: Giphy search text should show in context of giphy message

Repro:
- Use any device
- Send giphy message as user 1
- Reload the app as user 2
- Observed: The giphy search text looks like any other message, you can't tell that it was the text that prompted the giphy
- Expected: The giphy search text shows in the context of the giphy message

### Feature Request 4

Description: You should be able to substitute your own widget or text for the channel header name.  Our actual app is highly customized and we don't use
channel names in the same way as the demo apps.

### Feature Request 5

Description: You should be able to customize/remove the delay of the send message button transition.  I find this delay to be unacceptable from a UX
standpoint, it should at least be configurable / removeable.

### Feature Request 6

Description: This is more of an idea / suggestion than a specific feature request.  It would be nice to be able to somehow customize reactions.  Our app attempts to mimic slack reactions.
In order to do this in the old SDK I was forced to pull in and overwrite a lot of the stream widgets.
See screenshots/ReactionsExample.jpg for our current implementation with the old SDK


## Flutter Doctor (I use IntelliJ)

```
[✓] Flutter (Channel stable, 1.22.6, on macOS 11.2 20D64 darwin-x64, locale en-US)
    • Flutter version 1.22.6 at /Users/myname/flutter
    • Framework revision 9b2d32b605 (3 weeks ago), 2021-01-22 14:36:39 -0800
    • Engine revision 2f0af37152
    • Dart version 2.10.5
 
[✓] Android toolchain - develop for Android devices (Android SDK version 30.0.2)
    • Android SDK at /Users/myname/Library/Android/sdk/
    • Platform android-30, build-tools 30.0.2
    • ANDROID_HOME = /Users/myname/Library/Android/sdk/
    • Java binary at: /Users/myname/Library/Application Support/JetBrains/Toolbox/apps/AndroidStudio/ch-0/201.7042882/Android Studio.app/Contents/jre/jdk/Contents/Home/bin/java
    • Java version OpenJDK Runtime Environment (build 1.8.0_242-release-1644-b3-6915495)
    • All Android licenses accepted.

[✓] Xcode - develop for iOS and macOS (Xcode 12.3)
    • Xcode at /Applications/Xcode-beta.app/Contents/Developer
    • Xcode 12.3, Build version 12C5020f
    • CocoaPods version 1.10.0

[!] Android Studio (version 4.1)
    • Android Studio at /Applications/Android Studio.app/Contents
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
    • Java version OpenJDK Runtime Environment (build 1.8.0_242-release-1644-b3-6915495)

[✓] IntelliJ IDEA Ultimate Edition (version 2020.3.2)
    • IntelliJ at /Users/myname/Applications/JetBrains Toolbox/IntelliJ IDEA Ultimate.app
    • Flutter plugin installed
    • Dart plugin version 203.6912

[✓] VS Code (version 1.53.2)
    • VS Code at /Applications/Visual Studio Code.app/Contents
    • Flutter extension version 3.19.0

[✓] Connected device (2 available)
    • moto g 6 (mobile)      • ZY3232H6SC                • android-arm • Android 9 (API 28)
    • MyName's iPhone (mobile) • 00008030-000C05E60EF2802E • ios         • iOS 14.4

! Doctor found issues in 3 categories.
```