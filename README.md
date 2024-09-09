# rn-fetch-blog-repair

This is a modified version of the original [rn-fetch-blob](https://github.com/joltup/rn-fetch-blob) library, named `rn-fetch-blog-repair`, which includes a fix for compatibility with Android SDK 34 and above.

## About

`rn-fetch-blog-repair` is a React Native library used for network requests (like `fetch`) that also supports file system access and handling large binary files like images and videos.

This version was created to address an issue in the original `rn-fetch-blob` related to registering receivers for the `DownloadManager.ACTION_DOWNLOAD_COMPLETE` intent in Android SDK 34 and higher.

### Bug Description

In Android SDK 34, Google introduced stricter rules regarding the registration of broadcast receivers. When registering a receiver dynamically, either `Context.RECEIVER_EXPORTED` or `Context.RECEIVER_NOT_EXPORTED` must be specified if the receiver is not exclusively for system broadcasts. Failing to do so will result in the following error: `java.lang.IllegalArgumentException: One of RECEIVER_EXPORTED or RECEIVER_NOT_EXPORTED should be specified when a receiver isn't being registered exclusively for system broadcasts.` The original `rn-fetch-blob` library did not account for this requirement, which caused apps targeting Android SDK 34 or higher to crash when registering the receiver for the `DownloadManager.ACTION_DOWNLOAD_COMPLETE` intent.

### Changes from the Original Library

In the original `rn-fetch-blob` library, the following line:

```java
// appCtx.registerReceiver(this, new
// IntentFilter(DownloadManager.ACTION_DOWNLOAD_COMPLETE));
if (Build.VERSION.SDK_INT >= 34 && appCtx.getApplicationInfo().targetSdkVersion >= 34) {
    appCtx.registerReceiver(this, new IntentFilter(DownloadManager.ACTION_DOWNLOAD_COMPLETE),
            Context.RECEIVER_EXPORTED);
} else {
    appCtx.registerReceiver(this, new IntentFilter(DownloadManager.ACTION_DOWNLOAD_COMPLETE));
}
```

## Special Thanks

A special thanks to [rada](https://github.com/rada) for identifying and helping to fix the issue with broadcast receiver registration in Android SDK 34. Your contribution has been invaluable to this project!
