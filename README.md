RPOnvif - IN DEVELOPMENT STAGE -
RPOnvif will stream a Raspberry PI's Camera, Desktop, Slideshow, Picture, or a Webpage to a Network Video Recorder (NVR). RPOnvif will be a virtual ONVIF IP camera which will capture a Raspberry Pi's screen, and webcams among other things, then stream them as real-time live video feeds from a Raspberry Pi. Using the ONVIF standard means that RPOnvif will be compatible with many Network Video Recorders (NVR's).

Features - Autostart, Run in background, Auto discovery over the network. Stream the output of a browser / web page.

Streaming protocol / RTSP, RTP. Using GStreamer.

WEBKIT WPE-BASED WEB BROWSER SOURCE ELEMENT.

RTSP SERVER: ONVIF standard - Camera protocol / ONVIF , Profile S & T ready. ONVIF authentication / WS-UsernameToken.

The steps I took are as follows:

Starting off by installing latest OS on a Raspberry PI 3.

2019-09-26-raspbian-buster.img / "Raspbian GNU/Linux 10 (buster)"

To check what version OS is on your Raspberry PI.
```
$ cat /etc/os-release
```
PRETTY_NAME="Raspbian GNU/Linux 10 (buster)" NAME="Raspbian GNU/Linux" VERSION_ID="10" VERSION="10 (buster)" VERSION_CODENAME=buster ID=raspbian ID_LIKE=debian

If you want a bit more information, such as what linux kernel you are running, you can also try:
```
$ uname -a
```
Linux raspberrypi 4.19.75-v7+ #1270 SMP Tue Sep 24 18:45:11 BST 2019 armv7l GNU/Linux

STEP 1 - ENABLE RASPBERRY PI CAMERA

Run ‘sudo raspi-config’ enable the camera and reboot. Make sure your GPU memory is set to 128.

STEP 2 - INSTALL NODEJS AND NPM Assuming that node is NOT installed on your Raspberry Pi.
```
$ curl -L https://raw.githubusercontent.com/tj/n/master/bin/n -o n
$ sudo bash n lts
```
Make cache folder, and take ownership.
```
$ sudo mkdir -p /usr/local/n
$ sudo chown -R $(whoami) /usr/local/n
```
Take ownership of node and install destination folders.
```
$ sudo chown -R $(whoami) /usr/local/bin /usr/local/lib /usr/local/include /usr/local/share
$ Install n
```
```
$ npm install -g n
```
At this point the latest node and npm should be installed. Command for node is node –v and the command for npm is npm –v

At this point you can install whatever version of node you want by typing the following.
```
$ n 8
$ n 10
```
To switch between versions type “n” and enter. Arrow down or up to the version you want.

Currently I'm only using node v8.17.0 (with npm 6.13.4) or node v10.18.1 (with npm 6.13.4)

More information at https://github.com/tj/n

STEP 3 - GET RPOS SOURCE, AND INSTALL DEPENDENCIES
```
$ git clone https://github.com/BreeeZe/rpos.git
$ cd rpos
$ npm install
```
STEP 4 - COMPILE TYPESCRIPT(.ts) TO JAVASCRIPT(.js) using GULP While still in the rpos directory.
```
$ npx gulp
```
STEP 5 - INSTALL GSTREAMER USING APT:

While still in the rpos directory.
```
$ sudo apt install git gstreamer1.0-plugins-bad gstreamer1.0-plugins-base
$ sudo apt install git gstreamer1.0-plugins-good gstreamer1.0-plugins-ugly
$ sudo apt install git gstreamer1.0-tools libgstreamer1.0-dev libgstreamer1.0-0-dbg
$ sudo apt install git libgstreamer1.0-0 gstreamer1.0-omx
$ sudo apt install git libgstreamer-plugins-base1.0-dev gtk-doc-tools
```
STEP 6 INSTALL GST-RPICAMSRC FROM SOURCE
```
$ cd ..
$ git clone https://github.com/thaytan/gst-rpicamsrc.git
$ cd gst-rpicamsrc
$ ./autogen.sh
$ make
$ sudo make install
$ cd ..
```
Check successful plugin installation by executing
```
$ gst-inspect-1.0 rpicamsrc
```
STEP 7 - EDIT CONFIG

Edit rposConf.json to fit your application.

Options include:

Add a Username and Password for ONVIF access.
Change the TCP Port for the Camera configuration and the ONVIF Services.
Change the RTSP Port.
Enable PTZ support by selecting Pan-Tilt HAT or RS485 backends (Visca and Pelco D).
Enable multicast.
Switch to the mpromonet or GStreamer RTSP servers.
Hardcode an IP address in the ONVIF SOAP messages.
Initial setup is now complete!

STEP 8 - RUN RPOS.JS Launch RPOS to start the application:
```
$ cd rpos
$ node rpos.js
```
EXTRA CONFIGURATION ON PAN-TILT HAT The camera on the Pan-Tilt hat is usually installed upside down. Goto the Web Page that runs with //rpos http://:8081 and tick the horizontal and vertial flip boxes and apply the changes.

Compiled installation and script from the following.

TJ Holowaychuk AKA tj - https://github.com/tj Johnny Wan AKA johnnyxwan - https://github.com/johnnyxwan Jeroen Versteege AKA BreeeZe - https://github.com/BreeeZe
