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

6. For the remaining packages (`jna` and `jna-platform`), create a folder named `5.10.0` in each of these directories: `libraries/net/java/dev/jna/jna` and `libraries/net/java/dev/jna/jna-platform` (find it in your Minecraft directory). Then move the `jna` package to the former, and `jna-platform` to the latter directory.

7. Download the patched JSON file for LabyMod 1.8.9 in this repository and replace the original ones in the corresponding version directory.

8. Set these JVM arguments: (optional?)
```
-Djna.boot.library.path=${POJAV_NATIVEDIR} -Djna.nosys=true
```
Kindly check one of your `latestlog.txt`s to see what `${POJAV_NATIVEDIR}` is. Don't actually use `${POJAV_NATIVEDIR}` as the parameter, it may not work!

Once you have done these steps, voila! Your game should be able to launch!

Although this fix targets Discord RPC feature, it's very likely to not work with most of the other Discord RPC mods. For those mods, go to [COMING SOON!]

