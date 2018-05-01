The issues in this document apply only to Android 8.0 devices.

### Problem: _Device Not Detected by ADB after Updating to Android 8.0 on Windows 10_
- **Symptoms**
  - Device connects to PC, but does not show up in device list of the `adb devices` command
  - Occasional notifications on the device stating that it's unable to connect
- **Causes**
  - Allowing USB Debugging _does not_ carry over after updating to Android 8.0
  - The device effectively "forgot" the computer's RSA key fingerprint
- **Fixes**
  - _Update/Reset the corresponding device driver_
    - Connect the device to your machine
    - Locate the device in the Windows device manager
    - Right-click and select "update driver"
    - Once complete, you will need to restart your computer
    - After restarting, connect the device to your PC (ensure that developer mode and USB debugging is enabled on the device)
    - If the "Allow USB Debugging" prompt does not show up automatically after connecting the device:
      - Run the command `adb devices` while the device is connected - this should trigger the "Allow USB Debugging" prompt
      - Make sure you select "Always allow from this computer" if this is your main development machine