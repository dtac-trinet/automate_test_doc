# Administrator manual to jenkins-ci service

## Prepared dependencies
1. java jdk-8 and up
1. git-cli
1. python 3.6 and up
1. homebrew
1. xcode
1. xcode-cli
1. android studio

## Python library
all in latest version.  
1. requests
1. urllib
1. virtualenvwrapper
1. robotframework
1. robotframework-selenium2library
1. robotframework-appiumlibrary
1. robotframwork-pythonlibcore
1. pyOpenSSL

## How to start web service
- Open lauchpad > BrewServiceManubar  
    <img src="img/brewservicemenubar.png" alt="" width="750" height="">  
- In menubar > check `jenkins-lts`  and you can manage all service:
    <img src="img/brewjenkins.png" alt="" width="250" height="">
    - Start all  
    - Stop all  
    - Restart all  
- In terminal you can start ngrok services in backgroud process.
    - To start ngrok  
        ```bash
        # Start process in launchd
        $ launchctl load -w ~/Library/LaunchAgents/io.dtac.ngrok.plist
        ```
    - To stop ngrok  
        ```bash
        # Stop ngrok and unload from launchd
        $ launchctl unload -w ~/Library/LaunchAgents/io.dtac.ngrok.plist
        ```
- Open any browser and go to `https://dtac.ap.ngrok.io`, then it should redirect to github oauth api.  
<img src="img/jenkinslogin.png" alt="" width="450" height="">
- login with your account, so your browser callback to jenkins web-service

## Jenkins dashboard

### Robotframework test with iOS application
- Open job's folder `Robot-iOS`.
- See multi-branch builder in `Auto Build and Test with multiple branch`  
<img src="img/jenkins-multi-ios.png" alt="" width="750" height="">
    - In each branch pipeline test with latest iOS version(14.3)  
    <img src="img/jenkins-multi-ios-dashboard.png" alt="" width="750" height="">
    - If you re-build in branch, in left menu select `build now` 
    <img src="img/jenkins-buildnow.png" alt="" width="100" height="">
    - After pipeline success you can see summary report and relative with previous build in top page.  
    <img src="img/jenkins-robotgraph.png" alt="" width="450" height="">
    - Or see more detail for `robot results` with original report in left menu 
    <img src="img/jenkins-robotreport.png" alt="" width="100" height="">  
    <img src="img/jenkins-robotreport-detail.png" alt="" width="750" height="">  
- Or run CI with manaul selecte branch in `Pipeline Selected branch for manual Robot test`  
    - Click `Build with Parameters` to start full robot test.
    <img src="img/jenkins-buildwithparam.png" alt="" width="150" height="">
    - Select list of git branchs in **SELECTED_BRANCH** box and `build`.  
    <img src="img/jenkins-buildwithparam-box.png" alt="" width="450" height="">
    - See `robot results` with original report like above job.
  
    
### Robotframework test with Android application
- Open job's folder `Robot-iOS`.
- See multi-branch builder in `Auto Build and Test with multiple branch`  
<img src="img/jenkins-multi-android.png" alt="" width="750" height="">
    - In each branch pipeline test with latest android OS version(11)  
    <img src="img/jenkins-multi-android-dashboard.png" alt="" width="750" height="">
    - If you re-build in branch, in left menu select `build now` 
    <img src="img/jenkins-buildnow.png" alt="" width="100" height="">
    - After pipeline success you can see summary report and relative with previous build in top page.  
    <img src="img/jenkins-robotgraph.png" alt="" width="450" height="">
    - Or see more detail for `robot results` with original report in left menu 
    <img src="img/jenkins-robotreport.png" alt="" width="100" height="">  
    <img src="img/jenkins-robotreport-detail.png" alt="" width="750" height="">  
- Or run CI with manaul selecte branch in `Pipeline Selected branch for manual Robot test`
    - Click `Build with Parameters` to start full robot test.
    <img src="img/jenkins-buildwithparam.png" alt="" width="150" height="">
    - Select list of git branchs in **SELECTED_BRANCH** box and `build`.  
    <img src="img/jenkins-buildwithparam-box.png" alt="" width="450" height="">
    - See `robot results` with original report like above job.

## Support contact
Any problem in usage **jenkins**, you should contact Support Engineer in *line*.