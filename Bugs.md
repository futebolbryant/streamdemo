
Repro steps: Clean project, one channel, one user

======== Exception caught by rendering library =====================================================
A RenderFlex overflowed by 8.0 pixels on the right.
The relevant error-causing widget was:
Row file:///Users/johnbryant/flutter/.pub-cache/hosted/pub.dartlang.org/stream_chat_flutter-1.0.2-beta/lib/src/channel_list_view.dart:404:21
====================================================================================================




Android
Permissions problem ON FIRST LOAD ONLY
Start app, go to channel page, tap attachment icon
App crashes
Tap Allow
Restart app
Works fine :\
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


Android
OOM errors

```
W/Adreno-GSL( 2511): <sharedmem_gpuobj_alloc:2461>: sharedmem_gpumem_alloc: mmap failed errno 12 Out of memory
E/Adreno-GSL( 2511): <gsl_memory_alloc_pure:2236>: GSL MEM ERROR: kgsl_sharedmem_alloc ioctl failed.
W/Adreno-GSL( 2511): <sharedmem_gpuobj_alloc:2461>: sharedmem_gpumem_alloc: mmap failed errno 12 Out of memory
E/Adreno-GSL( 2511): <gsl_memory_alloc_pure:2236>: GSL MEM ERROR: kgsl_sharedmem_alloc ioctl failed.
```



Android on open/close attachments menu
Tap into message input, then tap attachments icon

```
======== Exception caught by rendering library =====================================================
The following assertion was thrown during layout:
A RenderFlex overflowed by 3.2 pixels on the bottom.

The relevant error-causing widget was:
Column file:///Users/johnbryant/Documents/Source/testing/streamdemo/lib/main.dart:116:13
The overflowing RenderFlex has an orientation of Axis.vertical.
The edge of the RenderFlex that is overflowing has been marked in the rendering with a yellow and black striped pattern. This is usually caused by the contents being too big for the RenderFlex.

Consider applying a flex factor (e.g. using an Expanded widget) to force the children of the RenderFlex to fit within the available space instead of being sized to their natural size.
This is considered an error condition because it indicates that there is content that cannot be seen. If the content is legitimately bigger than the available space, consider clipping it with a ClipRect widget before putting it in the flex, or using a scrollable container rather than a Flex, like a ListView.

The specific RenderFlex in question is: RenderFlex#047d2 relayoutBoundary=up1 OVERFLOWING
...  needs compositing
...  parentData: offset=Offset(0.0, 118.2); id=_ScaffoldSlot.body (can use size)
...  constraints: BoxConstraints(0.0<=w<=392.7, 0.0<=h<=413.8)
...  size: Size(392.7, 413.8)
...  direction: vertical
...  mainAxisAlignment: start
...  mainAxisSize: max
...  crossAxisAlignment: center
...  verticalDirection: down

```


Android, giphy search, doesn't close keyboard
Tap instant commands, search for giphy 'test' tap search icon
Keyboard stays open, you can't see the giphy search result
expected: keyboard closes after tapping search