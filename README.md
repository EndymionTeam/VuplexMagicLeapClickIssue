# Issue
On Magic Leap 2 device, `controller interactions` (such as hovering, clicking) are not working properly on Webviews. Most of the time, trying hovering/clicking using ML Controller doesn't produce any effect on webpage (instead the visible raycast from the controller change color from red to white when colliding with the webview). 
Rarely, at application startup, it starts working for a few interactions (hovering, clicking, dragging), but suddenly stop. Sometimes the application even crash.

Here an extract of stackatrace that seems relevant:
```
..
Info CameraManagerGlobal Connecting to camera service
Warn cr_media Requires MODIFY_AUDIO_SETTINGS and RECORD_AUDIO. No audio device will be available for recording
Warn System.err java.lang.SecurityException: getCameraCharacteristics:582: caller with invalid privileges (calling PID 7538, UID 10069) trying to access camera 2 
Warn System.err 	at android.hardware.camera2.CameraManager.throwAsPublicException(CameraManager.java:793)
Warn System.err 	at android.hardware.camera2.CameraManager.getCameraCharacteristics(CameraManager.java:327)
Warn System.err 	at WV.ZI.m(chromium-SystemWebView.apk-stable-609920407:15)
Warn System.err 	at org.chromium.media.VideoCaptureFactory.isLegacyOrDeprecatedDevice(chromium-SystemWebView.apk-stable-609920407:1)
Warn System.err 	at org.chromium.media.VideoCaptureFactory.getDeviceName(chromium-SystemWebView.apk-stable-609920407:1)
Warn System.err Caused by: android.os.ServiceSpecificException: getCameraCharacteristics:582: caller with invalid privileges (calling PID 7538, UID 10069) trying to access camera 2  (code 1)
Warn System.err 	at android.os.Parcel.createException(Parcel.java:2085)
Warn System.err 	at android.os.Parcel.readException(Parcel.java:2039)
Warn System.err 	at android.os.Parcel.readException(Parcel.java:1987)
Warn System.err 	at android.hardware.ICameraService$Stub$Proxy.getCameraCharacteristics(ICameraService.java:734)
Warn System.err 	at android.hardware.camera2.CameraManager.getCameraCharacteristics(CameraManager.java:316)
Warn System.err 	... 3 more
Fatal chromium [FATAL:jni_android.cc(290)] Please include Java exception stack in crash report
...
```

> Full stacktrace of the error is available [here](./stacktrace.md). This include all logging since the App has been launched.

# Environment

## Development 
- Unity: 2022.3.17f1 (for Windows)
- Vuplex: 4.7.1  (Android)
- Magic Leap Unity SDK: 2.2.0 (OpenXR)
- Magic Leap C SDK: 1.7.0
- ARFoundation: 5.1.1 (as a Magic Leap dependency)

## Device Specs
- Model: Magic Leap 2
- OS: Android AOSP 1.7.0

## Build
- Binary built both with and without `Development Build` option.

# Example
Reference scene is [Assets/Scenes/SampleScene](./Assets/Scenes/SampleScene.unity). The scene is setup to use XR Rig for Magic Leap, to create a Canvas Webview at startup, and to configure tracked device raycaster following [ Vuplex XR Example](https://github.com/vuplex/xrit-webview-example) and [Vuplex XR Guidelines](https://support.vuplex.com/articles/xr-interaction-toolkit)

# Steps 
Steps to reproduce the issue:
1. Add Vuplex library to Unity project
2. Install Magic Leap C SDK (from Magic Leap Hub desktop application)
3. Add Magic Leap Unity SDK (eventually found under Package/com.magicleap.unitysdk.tgz or using Magic Leap Setup Tool unity package)
3. Build for Android device
4. Launch the app. A canvas webview with google site will be opened.
5. Try to interact with page using the raycast controller.

> NOTE: Raycast interaction and Crash are very randomic. 