Well, that moment which 5 months ago I already knew that it must come. But I didn't expect that I could go from zero to hero that fast. Happy mining!

\- Hung, February 28 2022.



Oh Bois, that's LabyMod 1.8.9! It's once my priority but schoolwork takes all the place. But no matter, I got it!

Here's the instruction for it to work. If you come to this place in 2030 for example, the links may no longer work, please check for yourself.

This will only work in Action builds!

1. Download [the JNA package](https://repo1.maven.org/maven2/net/java/dev/jna/jna/5.10.0/jna-5.10.0.jar) and [the JNA platform package](https://repo1.maven.org/maven2/net/java/dev/jna/jna-platform/5.10.0/jna-platform-5.10.0.jar).

2. Download the native library: [64-bit](https://github.com/java-native-access/jna/blob/master/lib/native/android-aarch64.jar), [32-bit](https://github.com/java-native-access/jna/blob/master/lib/native/android-arm.jar), or [32-bit ARMv7](https://github.com/java-native-access/jna/blob/master/lib/native/android-armv7.jar). I still don't know which 32-bit library actually works, give both of them a try.

3. Extract what you have downloaded in step 2. Make sure there is a file called `libjnidispatch.so` in the package.

4. Download and install an APK editor. Just ask Google if you're in doubt.

5. Most of the APK editors will follow this procedure:

5.1. Tap "Select APK from app", choose "PojavLauncher (Minecraft: Java Edition for Android)" with the `net.kdt.pojavlaunch.debug` string beneath. Choose "Full edit", then "All files".

5.2. In the "Files" tab, navigate to `lib` > `arm64-v8a` (64-bit devices)/`armeabi-v7a` (32-bit devices). Delete the existing `libjnidispatch.so`, then tap "Add file", find the extracted `libjnidispatch.so` and choose to add it.

5.3. Tap "Build" to rebuild the APK. Once it's complete, install the APK.

[Visual example here.](https://youtube.com/shorts/Mmp9_Pq1AtM?feature=share)

6. For the remaining packages (`jna` and `jna-platform`), create a folder named `5.10.0` in each of these directories: `libraries/net/java/dev/jna/jna` and `libraries/net/java/dev/jna/jna-platform` (find it in your Minecraft directory). Then move the `jna` package to the former, and `jna-platform` to the latter directory.

[Visual example here.](https://youtube.com/shorts/QrHbk1CI3jY?feature=share)

7. Download the patched JSON file for LabyMod 1.8.9 in this repository and replace the original ones in the corresponding version directory.

8. Set these JVM arguments: (optional?)
```
-Djna.boot.library.path=${POJAV_NATIVEDIR} -Djna.nosys=true
```
Kindly check one of your `latestlog.txt`s to see what `${POJAV_NATIVEDIR}` is. Don't actually use `${POJAV_NATIVEDIR}` as the parameter, it will not work!

[Visual example here.](https://youtube.com/shorts/0EcYtft7DGY?feature=share)

Once you have done these steps, voila! Your game should be able to launch!

Although this fix targets Discord RPC feature, it's very likely to not work with most of the other Discord RPC mods. For those mods, go to [COMING SOON!]

### That doesn't work. What could be wrong?

- Android 6.0+ is the minimum requirement theoretically. If you are facing issues with Android 6.0, write a bug report with the `latestlog.txt` file attached.
- If the above requirement is passed, but the game still crashes after following, find the crash report written in `latestlog.txt`

#### Cannot obtain static method `<something>` from class `com.sun.jna.Native`

It indicates that no support for your device's architecture was found. Check if you have done step 7 correctly, as newer JNAs support more architectures.

#### (Android 7.0+ only) `<something>`: `dlopen` failed: `<something>` needed or `dlopen`ed by `<something>` is not accessible for the class loader `classloader-namespace`

This is the intentional behavior of Android 7.0+ that prevents executing native binaries on a "writable" app directory, which indicates that JNA didn't successfully find the executable on the desired directories, and fell back to loading its own executable.

To fix this, try doing step 5 and 8 again.

#### Crash on launch

Probably you should check your JVM options.
