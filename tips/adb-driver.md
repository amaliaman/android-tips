# ADB USB driver not recognized

If after following the instructions in https://developer.android.com/studio/run/oem-usb.html your phone still isn't recognized by Android studio, it means that the USB ADB driver isn’t installed properly on your computer.

You can verify it by browsing to *chrome://inspect/#devices* and NOT seeing your phone listed.

### This might help
Download Intel ADB drivers and follow the instructions in https://software.intel.com/en-us/xdk/docs/installing-android-debug-bridge-adb-usb-driver-on-windows.

### Note
This applies to Windows systems with Intel hardware.
