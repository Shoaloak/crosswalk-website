# Android target setup

Crosswalk applications will run on either a hardware or emulated Android device.

Any version of Android from 4.0 or higher should work.

## Android device

To deploy a Crosswalk application to an Android device, you need a working connection between it and your host computer. The simplest way is to connect them together via a USB cable.

To test whether the device has been detected, run the following command from a shell:

    > adb devices
    List of devices attached
    Medfield532DC30E	device

If the list of devices attached is empty, you may need to change the developer options on your device to enable USB debugging (*Settings* &gt; *Developer options* &gt; turn on *USB debugging*).

<h3 id="Fixing-device-access-issues-on-Linux">Fixing device access issues on Linux</h3>

In some cases, running `adb` as a non-root user on Linux may result in your devices not being detected:

    > adb devices
    List of devices attached

or detected with insufficient permissions:

    > adb devices
    List of devices attached
    ????????????	no permissions

(The latter seems to happen with older Android 4.0.* versions.)

In both cases, a work-around is to run the `adb` server as root:

    # kill any existing server instances
    > sudo <path to Android SDK>/platform-tools/adb kill-server

    # start the adb server as root
    > sudo <path to Android SDK>/platform-tools/adb start-server

    # check for devices (non-root user should be ok now)
    > adb devices
    List of devices attached
    HT23KW103989	device

## Android emulator

To test your application on Android platforms you don't own, the next best option is to use emulated devices. You can install these via the Android SDK.

1. Start the Android SDK Manager via the `android` command on Linux or by running `SDK Manager.exe` on Windows.

2. In the SDK Manager window, check the following box in the list:

        [ ] Android 4.3 (API 18)
            [x] Intel x86 Atom System Image

   If you want to test with other Android API versions, install the corresponding x86 system images.

3. On <strong>Windows only</strong>, use the SDK Manager to download HAXM as well:

        [ ] Extras
            [x] Intel x86 Emulator Accelerator (HAXM)

   This provides better graphics performance for emulated x86 devices running on Windows.

   Note that the SDK Manager downloads HAXM, but does not install it, so you need to find and install it yourself. The file you need is called `IntelHaxm.exe`.

4. After the selected packages are installed, set up an emulator image by running the AVD Manager:

        > android avd

5. Create a new image called <strong>Tablet</strong> and select the following options:

  <ul>
    <li><em>Target</em>: <strong>Android 4.3</strong></li>
    <li><em>CPU/ABI</em>: <strong>Intel Atom (x86)</strong> (if you only downloaded an x86 image, this will be selected automatically)</li>
    <li><strong>Use Host GPU</strong> (check the box)</li>
  </ul>

  The configuration should look something like this:

  <img src='/assets/emulator.png' style="display:block;margin:0 auto">

6. Launch the new emulator image from a shell:

        > emulator -avd Tablet

   You should be able to connect to any running images using adb, as for a hardware device:

        $ adb devices
        List of devices attached
        emulator-5554   device

The emulated Android device is now set up and ready to be used as a deployment target.
