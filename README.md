# Hide Fingerprint Binary
Magisk module to disable the fingerprint sensor on the Sony Xperia 1 III (LOS23/A16), maybe others.
This is not a fix, but a temporary workaround until someone can figure out how to make the sensor work reliably.
And even if you just wanna disable the sensor for whatever reason, this might not even be the best way to do it,
just an easy/convenient way (if you happen to have Magisk) as a result of frustration and desperation from someone (me) who's not that experienced with android development.


# Why?
The Sony Xperia 1 III's fingerprint sensor has been known to stop working for some reason (heat/load/hardware failure?), which can trigger a loop where apparently the system tries to restart the fingerprint service approximately every millisecond (that's 1000 times per second!), which leads to phone slowing down, high CPU usage, phone overheating and battery drain, the 4 horsemen of the appocalypse, even when the phone *seems* to be idle, but especially when other apps are running.

So in my case, when running this in ADB from my PC:
```
adb root
adb shell dmesg
```
I was getting this line spammed non-stop:
```
init: Control message: Could not find 'android.hardware.biometrics.fingerprint@2.1::IBiometricsFingerprint/default' for ctl.interface_start from pid: 721 (/system/system_ext/bin/hwservicemanager)
```
And in the logcat, I saw these:

```
725  9122 system W libc: Unable to set property "ctl.interface_start" to "android.hardware.biometrics.fingerprint@2.1::IBiometricsFingerprint/default": PROP_ERROR_HANDLE_CONTROL_MESSAGE (0x20)
725  9122 system I hwservicemanager: Tried to start android.hardware.biometrics.fingerprint@2.1::IBiometricsFingerprint/default as a lazy service, but was unable to. Usually this happens when a service is not installed, but if the service is intended to be used as a lazy service, then it may be configured incorrectly.
725   725 system I hwservicemanager: Since android.hardware.biometrics.fingerprint@2.1::IBiometricsFingerprint/default is not registered, trying to start it as a lazy HAL (if it's not configured to be a lazy HAL, it may be stuck starting or still starting).
```
```
2435  4118 system W HidlToAidlSensorAdapter: NoSuchElementException
2435  4118 system W HidlToAidlSensorAdapter: java.util.NoSuchElementException
2435  4118 system W HidlToAidlSensorAdapter: 	at android.os.HwBinder.getService(Native Method)
2435  4118 system W HidlToAidlSensorAdapter: 	at android.os.HwBinder.getService(HwBinder.java:93)
2435  4118 system W HidlToAidlSensorAdapter: 	at android.hardware.biometrics.fingerprint.V2_1.IBiometricsFingerprint.getService(IBiometricsFingerprint.java:74)
2435  4118 system W HidlToAidlSensorAdapter: 	at android.hardware.biometrics.fingerprint.V2_1.IBiometricsFingerprint.getService(IBiometricsFingerprint.java:84)
2435  4118 system W HidlToAidlSensorAdapter: 	at com.android.server.biometrics.sensors.fingerprint.hidl.HidlToAidlSensorAdapter.getIBiometricsFingerprint(HidlToAidlSensorAdapter.java:210)
2435  4118 system W HidlToAidlSensorAdapter: 	at com.android.server.biometrics.sensors.fingerprint.hidl.HidlToAidlSensorAdapter$$ExternalSyntheticLambda2.get(R8$$SyntheticClass:0)
2435  4118 system W HidlToAidlSensorAdapter: 	at com.android.server.biometrics.sensors.fingerprint.hidl.HidlToAidlSessionAdapter.setCallback(HidlToAidlSessionAdapter.java:227)
2435  4118 system W HidlToAidlSensorAdapter: 	at com.android.server.biometrics.sensors.fingerprint.hidl.HidlToAidlSessionAdapter.<init>(HidlToAidlSessionAdapter.java:58)
2435  4118 system W HidlToAidlSensorAdapter: 	at com.android.server.biometrics.sensors.fingerprint.aidl.AidlSession.<init>(AidlSession.java:48)
2435  4118 system W HidlToAidlSensorAdapter: 	at com.android.server.biometrics.sensors.fingerprint.hidl.HidlToAidlSensorAdapter.getSession(HidlToAidlSensorAdapter.java:182)
2435  4118 system W HidlToAidlSensorAdapter: 	at com.android.server.biometrics.sensors.fingerprint.hidl.HidlToAidlSensorAdapter.lambda$getFingerprintUpdateActiveUserClient$3(HidlToAidlSensorAdapter.java:272)
2435  4118 system W HidlToAidlSensorAdapter: 	at com.android.server.biometrics.sensors.fingerprint.hidl.HidlToAidlSensorAdapter.$r8$lambda$hzKCz0gy8JwjgnnYzUAAoIgGgh4(HidlToAidlSensorAdapter.java:0)
2435  4118 system W HidlToAidlSensorAdapter: 	at com.android.server.biometrics.sensors.fingerprint.hidl.HidlToAidlSensorAdapter$$ExternalSyntheticLambda3.get(R8$$SyntheticClass:0)
2435  4118 system W HidlToAidlSensorAdapter: 	at com.android.server.biometrics.sensors.HalClientMonitor.getFreshDaemon(HalClientMonitor.java:71)
2435  4118 system W HidlToAidlSensorAdapter: 	at com.android.server.biometrics.sensors.fingerprint.hidl.FingerprintUpdateActiveUserClient.startHalOperation(FingerprintUpdateActiveUserClient.java:123)
2435  4118 system W HidlToAidlSensorAdapter: 	at com.android.server.biometrics.sensors.fingerprint.hidl.FingerprintUpdateActiveUserClient.start(FingerprintUpdateActiveUserClient.java:110)
2435  4118 system W HidlToAidlSensorAdapter: 	at com.android.server.biometrics.sensors.BiometricScheduler.checkCurrentUserAndStartNextOperation(BiometricScheduler.java:329)
2435  4118 system W HidlToAidlSensorAdapter: 	at com.android.server.biometrics.sensors.BiometricScheduler$UserSwitchClientCallback.lambda$onClientFinished$0(BiometricScheduler.java:177)
2435  4118 system W HidlToAidlSensorAdapter: 	at com.android.server.biometrics.sensors.BiometricScheduler$UserSwitchClientCallback.$r8$lambda$9uToF41kHgR_T4eWN0T0wXyfa5w(BiometricScheduler.java:0)
2435  4118 system W HidlToAidlSensorAdapter: 	at com.android.server.biometrics.sensors.BiometricScheduler$UserSwitchClientCallback$$ExternalSyntheticLambda0.run(R8$$SyntheticClass:0)
2435  4118 system W HidlToAidlSensorAdapter: 	at android.os.Handler.handleCallback(Handler.java:995)
2435  4118 system W HidlToAidlSensorAdapter: 	at android.os.Handler.dispatchMessage(Handler.java:103)
2435  4118 system W HidlToAidlSensorAdapter: 	at android.os.Looper.loopOnce(Looper.java:248)
2435  4118 system W HidlToAidlSensorAdapter: 	at android.os.Looper.loop(Looper.java:338)
2435  4118 system W HidlToAidlSensorAdapter: 	at android.os.HandlerThread.run(HandlerThread.java:85)
2435  4118 system W HidlToAidlSensorAdapter: Fingerprint HAL not available
2435  4118 system E HidlToAidlSessionAdapter: Unable to set HIDL callback. HIDL daemon is null.
```
```
BiometricScheduler: [Client finished] {[105291] com.android.server.biometrics.sensors.fingerprint.hidl.FingerprintUpdateActiveUserClient, proto=1, owner=android, cookie=0, requestId=-1, userId=0}, success: false
BiometricScheduler: [Starting User] {[105292] com.android.server.biometrics.sensors.fingerprint.hidl.FingerprintUpdateActiveUserClient, proto=1, owner=android, cookie=0, requestId=-1, userId=0}
```

# How?

WARNING: This has only been tested specifically on a Sony Xperia 1 III (XQ-BC72) on [lineage-23.0-20251013-nightly-pdx215-signed](https://mirrorbits.lineageos.org/full/pdx215/20251013/lineage-23.0-20251013-nightly-pdx215-signed.zip) with [Magisk v30.4](https://github.com/topjohnwu/Magisk/releases/tag/v30.4). Not yet tested thoroughly and not guaranteed to work on any other hardware/software, so proceed at your own risk. If you're feeling adventurous (and desperate), you could check for fingerprint/biometrics-related files in your root/system folders, and modify my mobile zip by extracting it then add empty files with the same name and relative folder structure then zip back, flash and pray it doesn't brick your device.

Anyway, if you've come across this issue but haven't rooted your phone and want to fix it at all costs, it might be a good time to [do so](https://topjohnwu.github.io/Magisk/install.html).
Then, just download and flash the module ([zip file](https://github.com/ThreeDeeJay/hide-fingerprint-binary/releases/latest)) by selecting Install from storage and locate the Zip file.
<img width="405" height="155" alt="image" src="https://github.com/user-attachments/assets/e8df188b-c7d6-4c8c-b413-71aa2cc92f0a" />
<img width="391" height="199" alt="image" src="https://github.com/user-attachments/assets/efd44fbf-c210-493e-946a-ae02ec5482fe" />
Installing should just take a second and then you just gotta reboot, where you'll notice the fingerprint options (unlocking and system Settings options are gone).
The way this works is that Magisk adds a layer where it fakes the files that are actually stored in certain places, in this case the fingerprint sensor libraries, which are replaced with empty files on the surface, but without actually replacing the system files under the hood, which can trip system modification status and maybe even bootloops.

In my case, the improvement as shown in [3C Toolbox](https://play.google.com/store/apps/details?id=ccc71.at.free&hl=en-US)'s Task Manager as was night and day, with the phone running smootly and no more first degree burns.

<img width="400" height="326" alt="image" src="https://github.com/user-attachments/assets/fdae12ba-a03b-43d8-893a-671cf7553564" /> <img width="395" height="324" alt="image" src="https://github.com/user-attachments/assets/054d7f40-69ce-4283-928f-ebaba4dc9dca" />

# Credits
- [dartraiden/Fingerprint-Disabler](https://github.com/dartraiden/Fingerprint-Disabler) - Suggested more files I needed to hide.
