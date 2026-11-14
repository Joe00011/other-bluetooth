# Other Bluetooth

A tiny shim app for Fire TV that jumps straight to the
"Other Bluetooth Devices" screen hidden inside the system settings app.

## Why

Opening the Bluetooth settings on Fire TV takes many steps through the menus. This app opens the "Other Bluetooth Devices" screen directly, in a single step.

You can also combine it with an app that remaps remote buttons, such as Launcher Manager, to open the screen even more conveniently.

## Install

Download the APK from the [Releases](../../releases) page and sideload it
onto your Fire TV (e.g. via `adb install`).

## Build from source (optional)

Requires [apktool](https://ibotpeaches.github.io/Apktool/) and a JDK (keytool / jarsigner).

```bash
apktool b . -o other-bluetooth.apk

keytool -genkey -v -keystore debug.keystore -storepass android \
  -alias androiddebugkey -keypass android -keyalg RSA -keysize 2048 \
  -validity 10000 -dname "CN=Android Debug,O=Android,C=US"

jarsigner -verbose -sigalg SHA256withRSA -digestalg SHA-256 \
  -keystore debug.keystore -storepass android other-bluetooth.apk androiddebugkey

adb install other-bluetooth.apk
```

## Targeting a different screen

Edit this line in `smali/com/oss/firetv/blueshim/MainActivity.smali` to the
action you want, then rebuild:

```smali
const-string v1, "com.amazon.device.settings.action.INPUT_DEVICES"
```

The right action string can be found by decompiling the settings app
(`com.amazon.tv.settings.v2`) with apktool and inspecting the smali of the
Activity behind the screen you want.

## Notes

- Tested on: Fire TV Stick 4K (2nd Gen), firmware 8.1.5.5. Other devices and firmware versions are untested and may not work.
- The target screen is an internal Amazon settings action, so it may stop working after a system update.
- Unofficial. Not affiliated with or endorsed by Amazon. Amazon and Fire TV are trademarks of Amazon.com, Inc. or its affiliates.
- Use at your own risk.

## License

MIT

