# webvr-gearvr-test

[![experimental](http://badges.github.io/stability-badges/dist/experimental.svg)](http://github.com/badges/stability-badges)

<img src="http://i.imgur.com/jpwglKl.png" width="75%" />

A test compiling WebVR (with [A-Frame](https://aframe.io/)) to GearVR as a native application.

Unlike a simple web page, which requires [disabling GearVR service](https://www.reddit.com/r/WebVR/comments/3mt8t2/using_webvr_with_the_gear_vr_headset/) and setting up Chrome Dev, this approach intends to make a WebVR application that can be distributed through the Oculus Store. 

To do this, we compile with [Crosswalk](https://crosswalk-project.org/) to an Android APK prepared for the GearVR. See more details in my [`gearvr` fork](https://github.com/mattdesl/crosswalk-app-tools/tree/gearvr) of `crosswalk-app-tools`.

## Steps

> :bulb: This approach is still highly experimental. Any support/suggestions would be helpful.

First, you need the required Crosswalk dependencies, [see here](https://crosswalk-project.org/documentation/android.html). In short, you need `java`, `ant`, `python` and `adb` all set up and your environment variables correct. You also need the [Android SDK](http://developer.android.com/sdk/index.html#Other).

Next, you need the correct Android target to build Crosswalk (API Levle 21+). Run the following:

`android update sdk`

And ensure you have these installed:

<img src="http://i.imgur.com/woB8i1j.png" width="75%" /> 

Now you should be ready to clone this repo and install the npm deps.

```sh
git clone https://github.com/Jam3/webvr-gearvr-test.git
cd webvr-gearvr-test
npm install
```

Now you can start the app like so:

```sh
npm run start
```

And view `localhost:9966` to develop locally.

You can then build to an APK like so:

```sh
npm run build
```

This will generate an x86 and armeabi-v7a APK in the same directory. Assuming you are working on Samsung Galaxy S6, you can use `npm run push` to push the ARM APK to the device. 

Then you can install the APK with a File Manager application. When you open the app, it will prompt you to insert the phone into the GearVR (unless you are in [Oculus Developer Mode](https://developer.oculus.com/documentation/mobilesdk/latest/concepts/mobile-troublesh-device-run-app-outside/)).

See the [package.json `"scripts"`](./package.json) for details.

## Debugging

With Oculus Developer Mode, you will be able to remote debug the applications as you would a typical website.

## Roadmap

The [app/manifest.json](./app/manifest.json) file contains some details for the Crosswalk app. We can include a second command line switch, `--enable-webvr`, to enable Brandon Jones' experimental WebVR branch of chromium. However, it appears Crosswalk is still on an older version (Chrome v46), and throws an error in the `navigator.getVRDevices()` method, causing stereo rendering not to work correctly.

```
  "xwalk_command_line": "--ignore-gpu-blacklist --enable-webvr",
...
```

Once Crosswalk catches up to latest Chrome Dev (Android), we will hopefully have better head tracking than the built-in gyros. At a later point, it would be worth exploring native extensions or a custom build of Chromium which supports the Oculus Mobile SDKs.

## License

MIT, see [LICENSE.md](http://github.com/Jam3/webvr-gearvr-test/blob/master/LICENSE.md) for details.
