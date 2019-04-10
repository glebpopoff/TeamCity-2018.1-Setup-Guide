# TeamCity 2018.2.3 Linux (Ubuntu 18.04.2 LTS) Setup Guide
The guide provides instructions on how to setup TeamCity 2018.2.3 server running on Ubuntu 18.04.2 LTS server and configure build jobs for a TypeScript VueJS application and a .NET Core 2 Web API.

Note:
Windows Setup guide is here: https://github.com/glebpopoff/TeamCity-2018.1-Setup-Guide/blob/master/README.md
Having done both multiple times, I have to say that the Linux installation is a little simpler than Windows. Teamcity and Node appear to run faster on Windows (as far as the build time goes)

Requirements: 
- Linux distro. Suggested configuration: 4 vcpus, 16GB RAM
- TeamCity 2018.2.3
- Root-level access to the box via SSH
- No need for RDC/GUI access to the server - everything can be accomplished over SSH

We're trying to accomplish the following:
- DotNet core project build & publish (e.g. dotnet publish)
- TypeScript build (e.g tsc)
- VueJS distribution build (e.g. npm run build)
- Git-based deployment (e.g. git remote add & git push <remote>)

## Connect to the server as root
SSH to the box as a root user

## Install JRE 1.8
TeamCity 2018.2.3 requires a very specific JRE version (1.8) so you need to make sure you have it installed.
Run ```java -version``` from the command line to see what version you have pre-installed. 
If you do not have Java 1.8 installed, go ahead and install it:

Update your apt-get references:
```sudo apt-get update```

Then run:
```sudo apt-get install oracle-java8-installer```

If you're seeing the package not found errors, run this:

```
sudo apt-get install python-software-properties
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
```
Make sure you have the right version now: ```java -version```

## Install MySQL Server & Client
If you don't have mysql server/client installed, go ahead and install it
```sudo apt-get install mysql-server```

## Change MySQL Root Login
This is what worked for me (apparently, this is a little tricky with the latest version(s) of MySQL):
```
sudo mysql

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
```
Source: https://stackoverflow.com/questions/41645309/mysql-error-access-denied-for-user-rootlocalhost

## Download TeamCity
Run the following command:
```wget https://download.jetbrains.com/teamcity/TeamCity-2018.2.2.tar.gz```
If the link above doesn't work, make sure you have the right link

Untar and ungzip the file, then run *./runAll.sh start* from the bin folder:
```
cd TeamCity;

cd bin;

./runAll.sh start
```

## Install TeamCity 
Once you run the server and agent processes, you should be able to now open the browser and navigate to this url to install TeamCity:
http://*your server hostname*:8111/

If that doesn't work, check your firewall settings. If necessary, install XRDP (steps below) to try this on the server itself (you may need to install FireFox or another web browser).


## MySQL Storage
Make sure to specify MySQL as a back-end storage for TeamCity when prompted

## Install .NET Core 2.2
.NET Core runtime is required to build .NET core projects, follow the steps below to install it
```
sudo apt-key adv --keyserver packages.microsoft.com --recv-keys EB3E94ADBE1229CF
sudo apt-key adv --keyserver packages.microsoft.com --recv-keys 52E16F86FEE04B979B07E28DB02C46DF417A0893
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-bionic-prod bionic main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-get update
sudo apt-get install dotnet-sdk-2.2
```
Next, make sure to set *$DOTNET_HOME* environment variable (required by the TeamCity agent in order to build .NET core jobs):
```
export DOTNET_HOME="/usr/share/dotnet"
echo $DOTNET_HOME
export  PATH="$PATH:$DOTNET_HOME"
```

Source: https://dev.to/carlos487/installing-dotnet-core-in-ubuntu-1804-7lp

## Install Node, NPM, and TypeScript
Run these commands to install the required packages:
```
sudo apt-get install nodejs
sudo apt-get install npm
npm install -g typescript
```

## Update TimeZone / Date & Time
If server date/time is out of sync or on a different timezone, you may want to change it:
```
date
tzselect
sudo dpkg-reconfigure tzdata
date
```

## RDC / XRDP Access (optional)
If you want to have XRDP access to your server, follow the steps below to get it installed and working. You can use a regular MS RDC client app (e.g. Mac or Windows) to connect to your server:
```
sudo apt-get update
sudo apt-get install xrdp
sudo apt-get update
sudo apt-get install xfce4
echo xfce4-session > .xsession
```
Source: https://askubuntu.com/questions/464389/xrdp-gray-screen-tried-everything

## Kill Hanging Dotnet Build Processes (optional)
For some reason the dotnet build processes remain in the memory after each build. If that's the case in your environment, run ```ps aux | grep dotnet``` to see if you have this issue, schedule a quick cron job to kill them.
Example:
```
0 2 * * * pkill -f dotnet
```

## TeamCity Project and Build Setup
Refer to: https://github.com/glebpopoff/TeamCity-2018.1-Setup-Guide/blob/master/README.md
