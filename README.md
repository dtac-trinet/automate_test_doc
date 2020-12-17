# Manual

## APK download link
Android: https://install.appcenter.ms/users/mobileapp-yw2v/apps/dtac-dev/distribution_groups/automationtest  
iOS: https://install.appcenter.ms/users/mobileapp-yw2v/apps/dtac-beta/distribution_groups/dtacbeta  

## Run Robot Test in Android Steps
- Start appium
```bash
$ appium -p 4723 && appium -p 4725
```
- Move current dir to emulator workspace to use library qt
```bash
$ cd $(dirname $(which emulator))
```
- Get name of devices to run emulate
```bash
$ ./emulator -list-avds
```
- Start emulator with device name in below output
```bash
$ ./emulator @<device_name>
## Example
$ ./emulator @Nexus_5x86
```
