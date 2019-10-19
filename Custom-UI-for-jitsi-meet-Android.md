
# Custom UI for jitsi meet app   
  
This guide describes a setup to build a modified version of the Jitsi Meet app for android.  

I used ubuntu server 18.04.3 lts
commands are to be run as a sudo user  
  

we install npm I'm going to also install n (If you don't do this you still have to make sure you have npm 6+ and node 10+)  
  

```  
sudo apt -y install npm  
```  

## Now we follow the instructions to install n  

https://github.com/tj/n  

```  
sudo npm install -g n  
```  

and so sudo npm install isn't necessary  

```  
# make cache folder (if missing) and take ownership  
sudo mkdir -p /usr/local/n  
sudo chown -R $(whoami) /usr/local/n  
# take ownership of node install destination folders  
sudo chown -R $(whoami) /usr/local/bin /usr/local/lib /usr/local/include /usr/local/share  
```  
  

### more requirements for our dev environment 

#### If you don't already have a desktop environment we will install one now.   

``` 
## i put in the extra # so if you already have a desktop you don't  unnecessarily install another one.
#sudo apt -y install xfce4   
## startx starts your desktop enviroment  
#startx   
```  

You can use tightvnc if your using a vps.  
also if your using virtual box you should install your guest additions.  
  
  

fs.inotify.max_user_watches is definitely required if your not using watchman (It may be even if you are I'm not sure) https://github.com/gatsbyjs/gatsby/issues/11406#issuecomment-458769756  

```  
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p  
#jdk8 is required  
sudo apt -y install openjdk-8-jdk openjdk-8-jre  
```  

Now we install node 10   

```  
# As of 10-18-2019 this installs npm 6.9.0 and node v10.16.3  
# it would be `sudo n 10` or 10.16.3 or whatever otherwise   
sudo n lts  
sudo npm install -g react-native-cli  
```  
## We follow the instructions to install React Native CLI  


If you hollow this link neither react native quick start (nor linux) is not selected by default   
this is a slightly condensed version of the React Native CLI Quickstart
https://facebook.github.io/react-native/docs/getting-started.html  
Also I'm skipping the watchman install  


  #### 1. Install Android Studio

[Download and install Android Studio](https://developer.android.com/studio/index.html). Choose a "Custom" setup when prompted to select an installation type. Make sure the boxes next to all of the following are checked:

-   `Android SDK`
-   `Android SDK Platform`
-   `Android Virtual Device`
  Then, click "Next" to install all of these components.
> If the checkboxes are grayed out, you will have a chance to install these components later on.


#### 2. Install the Android SDK

The SDK Manager can be accessed from the "Welcome to Android Studio" screen. Click on "Configure", then select "SDK Manager".

> The SDK Manager can also be found within the Android Studio "Preferences" dialog, under  **Appearance & Behavior**  →  **System Settings**  →  **Android SDK**.

Select the "SDK Platforms" tab from within the SDK Manager, then check the box next to "Show Package Details" in the bottom right corner. Look for and expand the  `Android 9 (Pie)`  entry, then make sure the following items are checked:

-   `Android SDK Platform 28`
-   `Intel x86 Atom_64 System Image`  or  `Google APIs Intel x86 Atom System Image`
>I used Intel x86 Atom_64 System Image 

Next, select the "SDK Tools" tab and check the box next to "Show Package Details" here as well. Look for and expand the "Android SDK Build-Tools" entry, then make sure that  `28.0.3`  is selected.

Finally, click "Apply" to download and install the Android SDK and related build tools.
#### 3. Configure the ANDROID_HOME environment variable
Add the following lines to your  `$HOME/.bash_profile`  or  `$HOME/.bashrc`  config file:

```
export ANDROID_HOME=$HOME/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

Type  `source $HOME/.bash_profile`  to load the config into your current shell. Verify that ANDROID_HOME has been added to your path by running  `echo $PATH`.

> Please make sure you use the correct Android SDK path. You can find the actual location of the SDK in the Android Studio "Preferences" dialog, under  **Appearance & Behavior**  →  **System Settings**  →  **Android SDK**.
***
 ## Clone repo build and run
now clone you the git hub repo or your fork (or duplicate) of it.  
change directory to where you want to put your repo
```git clone https://github.com/your_github/your_fork_of_jitsi-meet.git```  
you want to fork (or you could just clone https://github.com/jitsi/jitsi-meet.git  

now cd into your project and npm install
```  
cd your_fork_of_jitsi-meet
npm install  
```  


enable debugging on your android device (or vm) and connect it  
if conecting over a network  
`adb connect $IP_OF_ANDROID `
or over usb (you may have to run this with sudo):
```adb usb```
  

```  
react-native run-android
react-native start  
```  

"react-native start" has to run in the background while you use the debug version of your app. 
or to have it reload on changes `react-native start --watchFolders .`  
you can run `adb uninstall org.jitsi.meet` to uninstall a different version of the app 
<br>  
Now your ready to start making your changes to jitsi for react native.
 
If you don't know where to start you can edit stuff in these files.   
>react/features/welcome/components/WelcomePage.native.js
	react/features/chat/components/native/Chat.js
	react/features/chat/components/native/ChatMessage.js
***  
  
  

> Written with [StackEdit](https://stackedit.io/).  


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NjUzMTQ2OTZdfQ==
-->