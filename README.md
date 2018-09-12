# TeamCity 2018.1 Setup Guide
The guide provides instructions on how to setup TeamCity 2018.1 server running on Windows 2016 server and configure build jobs for a VueJS application (with TypeScript) and a .NET Core 2 Web API.

Requirements: 
- Windows Server 2016 (lower versions will probably work as well)
- TeamCity 2018.1
- Full admin access to the server (including an ability to create new local-admin users)

## Create a New Local Admin User
It's probably best for the TeamCity process to run as a separate user vs a system user. The reason for that has to do with TeamCity process running various build commands, such as ```npm``` or ```git``` that will be required during the build phase.

The system user may not be able to run these commands (at least I wasn't able to get it to work). Skip this step if you're feeling adventurous and would like to tackle this on your own. 

If you're creating a new user, make sure the user is a) Local Admin and b) Allowed RDC connections (you'll connect to the server as that user to install various tools (e.g. npm, npm packages, git)

## Install Chrome, NotePad++
If it's a fresh server environment (e.g. Windows Server 2016), go ahead and install Chrome and NotePad++ to make your life easier troubleshooting things.

Chrome: https://www.google.com/chrome/browser/desktop/index.html
NotePad++: https://notepad-plus-plus.org/download/v7.5.8.html

## Download TeamCity 2018.1
Download the installer from this URL: https://www.jetbrains.com/teamcity/download

The installer for the current version (2018.1) is quite large ~ about 1GB in size.

Disable Windows Defender / scanner - 
