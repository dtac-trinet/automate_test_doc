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
    - Start all  
    - Stop all  
    - Restart all  
<img src="img/brewjenkins.png" alt="" width="450" height="">
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

