# Manual

## APK download link
Android: https://install.appcenter.ms/users/mobileapp-yw2v/apps/dtac-dev/distribution_groups/automationtest  
iOS: https://install.appcenter.ms/users/mobileapp-yw2v/apps/dtac-beta/distribution_groups/dtacbeta  

## Compile and Build app before simulate
### For iOS
move current path to app's repository
```bash
$ cd ~/robot_app/${repo_name}
```
Checkout branch to run test(in case use `develop` branch)
```bash
$ git checkout develop
```
Create fastlane script for test and build app
```ini
# This is the minimum version number required.
# Update this, if you use features of a newer version
fastlane_version "1.82.0"

default_platform :ios

platform :ios do
  desc "Build artifact file with ipa only, Not push to anywhere"
  desc "Use this step for local test."
  lane :jenkinsci do 

    # clear derived data
    clear_derived_data

    run_tests(
      workspace: "dtac-iservice.xcworkspace",
      devices: ["iPhone 11 Pro Max"],
      clean: true,
      skip_slack: true,
      output_directory: "./TestResult",
      derived_data_path: "./DerivedData",
      force_quit_simulator: true,
      scheme: "dtac-iservice"
    )
  end
end
```
Prepare Podfile
```bash
$ pod repo update
$ pod install
```
Run fastlane script
```bash
$ fastlane jenkinsci
# Artifact store in Derived dir.
# ${PWD}/DerivedData/Build/Products/Debug-iphonesimulator/dtac.app
```


## Prepare Android Steps
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
## Example for optimize boot time
$ ./emulator @Nexus_5_API_28 -no-boot-anim -no-window
```

## Prepare iOS Step
list device available to simulate
```bash
$ xcrun simctl list
```
Clean simulator before run test
```bash
$ xcrun simctl shutdown all
```
Start simulator
```bash
# find available device_id to start simulator
$ xcrun simctl list devices available
# use previous id
$ xcrun simctl boot <device_id>
$ open simulator -CurrentDeviceUDID <device_id>
```
Install app to simulator
```bash
$ xcrun simctl install DerivedData/Build/Products/Debug-iphonesimulator/dtac.app
```

## Run robot test
- move current dir to repo
```bash
$ cd ~/robot_app/AndroiddtacappAutomateTest/testcase
```
