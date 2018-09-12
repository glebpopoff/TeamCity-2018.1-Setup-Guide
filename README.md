# TeamCity 2018.1 Setup Guide
The guide provides instructions on how to setup TeamCity 2018.1 server running on Windows 2016 server and configure build jobs for a VueJS application (with TypeScript) and a .NET Core 2 Web API.

Requirements: 
- Windows Server 2016 (lower versions will probably work as well). Suggested configuration: 2 vcpus, 4GB RAM
- TeamCity 2018.1
- Full admin access to the server (including an ability to create new local-admin users)

## Create a New Local Admin User
It's probably best for the TeamCity process to run as a separate user vs a system user. The reason for that has to do with TeamCity process running various build commands, such as ```npm``` or ```git``` that will be required during the build phase.

The system user may not be able to run these commands (at least I wasn't able to get it to work). Skip this step if you're feeling adventurous and would like to tackle this on your own. 

If you're creating a new user, make sure the user is a) Local Admin and b) Allowed RDC connections (you'll connect to the server as that user to install various tools (e.g. npm, npm packages, git)

Using the Windows Server User Control panel, you can make the user an admin by changing the profile's user type, e.g. :
![Local User Type](https://raw.githubusercontent.com/glebpopoff/TeamCity-2018.1-Setup-Guide/master/local-user-type.png)

## Install Chrome, NotePad++
If it's a fresh server environment (e.g. Windows Server 2016), go ahead and install Chrome and NotePad++ to make your life easier troubleshooting things.

Chrome: https://www.google.com/chrome/browser/desktop/index.html
NotePad++: https://notepad-plus-plus.org/download/v7.5.8.html

If you using the server, make Chrome your default browser so that you don't have to deal with IE 11 nonsense. 

## Download TeamCity 2018.1
Download the installer from this URL: https://www.jetbrains.com/teamcity/download

The installer for the current version (2018.1) is quite large ~ about 1GB in size.

## Download MySQL Community Edition 8.X
TeamCity will need a database engine to persist its data and configuration. Skip this step if you have access to a RDBMS (MySQL, SQL Server, Postgres) - double check what's supported.

I'm going to use MySQL Community Server 8.0.12 for the setup. Download link: https://dev.mysql.com/downloads/windows/installer/8.0.html Make sure to choose the Installer Option (vs ZIP Archive). You can skip the login (the link is at the bottom) and just proceed to the Download screen. The file should be about 273MB.

## Start TeamCity Installation
Run the installer. Choose the "windows service" option for both agent and server selections along with the default port and other options. 

When prompted to save user configuration, go ahead and click the "Save" button - you should see a confirmation
![Save Agent Configuration](https://raw.githubusercontent.com/glebpopoff/TeamCity-2018.1-Setup-Guide/master/save-agent-props.png)

When the installer prompts you to select Service Account for the server select SYSTEM account for time being (we can switch later to the user created above). For some reason the installer didn't recognize the newly created user from the above step.

Once the installer finishes, the browser should open the following page: http://localhost/mnt to start the configuration wizard:
![Web Configuration Screen](https://raw.githubusercontent.com/glebpopoff/TeamCity-2018.1-Setup-Guide/master/teamcity-web-configuration.png)

If for some reason the installer fails or the page doesn't open, try re-installing teamcity from the beginning. If you can't get to the TeamCity First Start page (http://localhost/mnt), something went wrong.



## TeamCity Configuration
Using the TeamCity Web First Start page (http://localhost/mnt) go through the steps until you need to setup Database connection. You will be presented with the following screen:

![Database Setup](https://raw.githubusercontent.com/glebpopoff/TeamCity-2018.1-Setup-Guide/master/database-setup.png)

You can use Internal storage (HSQLDB) for like dev purposes but I'm going to select the MySQL option (assuming no other database service is available to us). I installed and used TeamCity with MySQL and didn't see any issues - that's the recommendation I'm going to make to you guys.

Select MySQL. Click on the 'Download JDBC Driver' button. We're not ready to setup MySQL. Keep the window open while starting  the MySQL installer.

## MySQL Setup
Run the MySQL Community Server installer and choose "Server" - this will install just the server. In my opinion, it's also good install the client software (Workbench or command line) so you can manage the server and maintain databases/users/privileges.

If the installer needs Microsoft Visual C++ 2015 Redistributable Package, download it from here: https://www.microsoft.com/en-us/download/confirmation.aspx?id=53587 , install it, restart the MySQL installer.

Choose the default options during the server setup, including the standard MySQL port (3306). Setup a strong MySQL root password. Setup a 'teamcity' user (DB admin). Make sure the server installs successfully.










Run the installer.



Do not run the installer yet. We need to download and install MySQL database 


Disable Windows Defender / scanner - 
