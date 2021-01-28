# Manual for run robot test with simulator

## Beta APP download link
Android: https://install.appcenter.ms/users/mobileapp-yw2v/apps/dtac-dev/distribution_groups/automationtest  
iOS: https://install.appcenter.ms/users/mobileapp-yw2v/apps/dtac-beta/distribution_groups/dtacbeta  

## Project repository
Mobile source code
1. [Dtac android mobile](https://github.com/dtacmobile/Androidtacmobile) 
1. [Dtac iOS mobile](https://github.com/dtacmobile/iOSdtacmobile)  
  
Robot UI test repository
1. [Robot for android](https://github.com/RAGOpoR/AndroiddtacappAutomateTest)
1. [Robot for iOS](https://github.com/RAGOpoR/iOSdtacappAutomateTest)

## Other Documents
1. [How to start jenkins](Jenkins.md)

## Compile and Build app before simulate
### For iOS
move current path to app's repository
```bash
repo_name="iOSdtacmobile"
$ cd ~/robot_app/${repo_name}
```
Checkout branch to run test(in case use `develop` branch)
```bash
BRANCH_NAME=develop # choose your branch name that you want to test ex. feature/jenkinsci , release/9.0.0
$ git checkout ${BRANCH_NAME}
# Example
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
### For Android
move current path to app's repository
```bash
repo_name=Androidtacmobile
$ cd ~/robot_app/${repo_name}
```
Checkout branch to run test(in case use `develop` branch)
```bash
BRANCH_NAME=develop # choose your branch name that you want to test ex. feature/jenkinsci , release/9.0.0
$ git checkout ${BRANCH_NAME}
# Example
$ git checkout develop
```
Change JAVA_HOME to jdk8 and build with fastlane
```bash
$ JAVA_HOME=JAVA_HOME=/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home fastlane beta
# Artifact store in Derived dir.
# ${PWD}/app/build/outputs/apk/prod/release/*.apk
```

## Setup Emulator
1. Open android studio  
1. Open AVD manager with: **Configure > AVD Manager**  
1. Open **+ Create Virtual Device**
1. Choose a device definition with: **Category Phone | Select your device interface > Next**
1. Select a system image: **Select your android alias name only x86**
1. Verify Configuration: **Insert AVD name > Check emulator configure > Finish**

## Prepare Android Emulator Steps
Start appium
```bash
$ appium -p 4723 && appium -p 4725
# first process for connect to app and second port for receive otp
```
Move current dir to emulator workspace to use library qt
```bash
$ cd $(dirname $(which emulator))
```
Get name of devices to run emulate
```bash
$ ./emulator -list-avds
```
Start emulator with device name in below output
```bash
$ ./emulator @<device_name>
## Example for optimize boot time
$ ./emulator @Nexus_5_API_28 -no-boot-anim -no-window
```
Clean package installed to refresh virtual test environment
```bash
$ adb uninstall $(adb shell pm list packages|grep th.co|awk '{split($0,array,":")} END{print array[2]}')
```
Install current app to emulate
```bash
$ adb install -r $(find $PWD/app/build/outputs/apk/prod/release/ -iname "*.apk")
```

## Prepare iOS Simulator Step
Start appium
```bash
$ appium -p 4723 && appium -p 4725
# first process for connect to app and second port for receive otp
```
list device available to simulate
```bash
$ xcrun simctl list
```
Open Application `Simulator`
```bash
$ open /Applications/Xcode.app/Contents/Developer/Applications/Simulator.app
```
Clean simulator before run test
```bash
$ xcrun simctl shutdown all
$ xcrun simctl erase all
```
Start simulator
```bash
# find available device_id to start simulator
$ xcrun simctl list devices available
# use previous id
$ xcrun simctl erase <device_id>
$ xcrun simctl boot <device_id>
```
Install app to simulator
```bash
# See app path 
$ xcrun simctl install DerivedData/Build/Products/Debug-iphonesimulator/dtac.app
```


## Robot variable
We show list of variable must inject on running test  
| Variable name | Description                                            | Required | Value type | Example                                                                      | Multiple value |
|---------------|--------------------------------------------------------|----------|------------|------------------------------------------------------------------------------|----------------|
| ar_Porturl    | Appium url endpoint with port that provide to emulator | yes      | String     | http://127.0.0.1:4723/wd/hub                                                 | no             |
| ar_OS         | Specified Simulator OS type                            | yes      | String     | iOS or Android                                                               | no             |
| ar_pfversion  | Version API of device's OS                             | yes      | Number     | 8.1                                                                          | no             |
| ar_devicename | Name of real device to used. In simulator not use.     | no       | String     | iPhone, Nexus5, VIVO                                                         | no             |
| ar_Lag        | Support language                                       | yes      | String     | EN                                                                           | no             |
| ar_udid       | Specified ID of simulator in used                      | yes      | String     | # for android Nexus_5X_API_30 # for iOS 44752AA7-1E63-412E-AA80-9342438D7302 | no             |

## Testcase Tag
For iOs
| Tag                                | Test case                                                                                                                    |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| CLIPostpaid                        | Login with Cellular Network (Postpaid)                                                                                       |
| CLIPrepaid                         | Login with Cellular Network (Prepaid)                                                                                        |
| phonePostpaid                      | Login with OTP (Postpaid), New Facebook Login – Add New Number (Postpaid), Old Facebook Login – Add New Number (Postpaid)    |
| phonePrepaid                       | Login with OTP (Prepaid), New Facebook Login – Add New Number (Prepaid), Old Facebook Login – Add New Number (Prepaid)       |
| FarmPostpaid                       | New Facebook Login – Number Paired (Postpaid), Old Facebook Login – Number Paired (Postpaid)                                 |
| FarmPrepaid                        | New Facebook Login – Number Paired (Prepaid), Old Facebook Login – Number Paired (Prepaid)                                   |

For android
| Tag                                | Test case                                                                                                                    |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| CLIPostpaid                        | Login with Cellular Network (Postpaid)                                                                                       |
| CLIPrepaid                         | Login with Cellular Network (Prepaid)                                                                                        |
| phonePostpaid                      | Login with OTP (Postpaid), New Facebook Login – Add New Number (Postpaid), Old Facebook Login – Add New Number (Postpaid)    |
| phonePrepaid                       | Login with OTP (Prepaid), New Facebook Login – Add New Number (Prepaid), Old Facebook Login – Add New Number (Prepaid)       |
| phonePostPermission                | Login Facebook but not bind Permission – Add New Number (Postpaid)                                                           |
| phonePrePermission                 | Login Facebook but not bind Permission – Add New Number (Prepaid)                                                            |
| phonePostLogout                    | Facebook is logout but remembered the account – Add New Number (Postpaid)                                                    |
| phonePreLogout                     | Facebook is logout but remembered the account – Add New Number (Prepaid)                                                     |
| FarmPostpaid                       | New Facebook Login – Number Paired (Postpaid), Old Facebook Login – Number Paired (Postpaid)                                 |
| FarmPrepaid                        | New Facebook Login – Number Paired (Prepaid), Old Facebook Login – Number Paired (Prepaid)                                   |
| FarmPostPermission                 | Login Facebook but not bind Permission – Number Paired (Postpaid)                                                            |
| FarmPrePermission                  | Login Facebook but not bind Permission – Number Paired (Prepaid)                                                             |
| FarmPreLogout                      | Facebook is Logout but remembered the account – Number Paired (Prepaid)                                                      |
| FarmPostLogout                     | Facebook is logout but remembered the account – Number Paired (Postpaid)                                                     |

## Example command test with farm test case
move current dir to repo
```bash
repo_name=AndroiddtacappAutomateTest
$ cd ~/robot_app/${repo_name}
```
  
For andriod
```bash
$ robot --nostatusrc -d farm-Nexus5X \
-v ar_Porturl:http://127.0.0.1:4723/wd/hub \
-v ar_OS:Android \
-v ar_pfversion:11.0.0 \
-v ar_devicename:SamsungS20 \
-v ar_Lag:EN \
-v ar_udid:Nexus_5X_API_30 \
-i Farm*
testcase/Login-Farm.robot
```
   
move current dir to repo
```bash
repo_name=iOSdtacappAutomateTest
$ cd ~/robot_app/${repo_name}
```
  
For iOS
```bash
$ robot --nostatusrc -d farm-iPhoneXr \
-v ar_Porturl:http://127.0.0.1:4723/wd/hub \
-v ar_OS:iOS \
-v ar_pfversion:12.5 \
-v ar_devicename:iPhoneXr \
-v ar_Lag:EN \
-v ar_udid:44752AA7-1E63-412E-AA80-9342438D7302 \
-i Farm*
testcase/Login-Farm.robot
```

## Maintainer
Refer to [MAINTAINER.md](MAINTAINER.md)

## License
Source code under: copyright © 2021 OnionShack
