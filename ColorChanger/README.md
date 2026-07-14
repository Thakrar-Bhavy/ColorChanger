# Color Changer

A single-Activity Android app (Kotlin, Material Design). Tap the "Change Color"
button in the center of the screen and the background switches to a random color.

- minSdk: 21
- targetSdk / compileSdk: 34
- One Activity: `MainActivity`
- Material Components theme + `MaterialButton`
- View binding, no unnecessary dependencies

## Project structure

```
ColorChanger/
├── app/
│   ├── build.gradle
│   ├── proguard-rules.pro
│   └── src/main/
│       ├── AndroidManifest.xml
│       ├── java/com/example/colorchanger/MainActivity.kt
│       └── res/
│           ├── layout/activity_main.xml
│           ├── values/ (strings, colors, themes)
│           ├── values-night/themes.xml
│           ├── mipmap-*/ic_launcher.png (+ round)
│           ├── mipmap-anydpi-v26/ (adaptive icon XML)
│           ├── drawable/ic_launcher_background.xml, ic_launcher_foreground.xml
│           └── xml/backup_rules.xml, data_extraction_rules.xml
├── build.gradle
├── settings.gradle
├── gradle.properties
├── gradlew / gradlew.bat
└── gradle/wrapper/gradle-wrapper.properties
```

## Building the signed debug APK

I could not run the actual Gradle/Android build in the sandbox this project
was generated in — it has no network access to `dl.google.com` /
`maven.google.com` (Android SDK + AndroidX artifacts) or `services.gradle.org`
(Gradle distribution), so `./gradlew` can't download what it needs there.
Everything else about the project is complete and standard, so building it
on a normal machine is just the usual steps:

### Option A: Android Studio (easiest)
1. Open Android Studio → **Open** → select the `ColorChanger` folder.
2. Let Gradle sync (it will download the wrapper, AGP, Kotlin plugin, and
   AndroidX/Material dependencies automatically).
3. **Build → Build Bundle(s) / APK(s) → Build APK(s)**.
4. Debug APKs are auto-signed with the Android debug keystore — no extra
   signing step needed. Output lands in `app/build/outputs/apk/debug/app-debug.apk`.

### Option B: Command line
Requires a JDK 17+ and the Android SDK command-line tools installed, with
`ANDROID_HOME` set (or an `local.properties` file with `sdk.dir=...`).

```bash
cd ColorChanger
./gradlew assembleDebug
# APK will be at:
# app/build/outputs/apk/debug/app-debug.apk
```

That debug APK is already signed (Gradle signs debug builds automatically
with the default debug keystore at `~/.android/debug.keystore`), so it's
ready to install with:

```bash
adb install app/build/outputs/apk/debug/app-debug.apk
```

## If you'd rather I build the APK for you
I can't reach the Android SDK/Gradle servers from this sandbox, but if you
have Claude Code, Cowork, or any environment with normal internet access,
you can point it at this same project and run `./gradlew assembleDebug` —
it should compile cleanly with no changes needed.
