# PrivacyPi

Inspired by Gavin's work at http://www.privacypi.org/ I've created this tutorial to make it as easy for anyone with basic computer skills to buy an off-the-shelf parts and make a device that increases the cost of government invasion of privacy from tens of dollars to tens of thousands of dollars.

Please submit issues or pull requests at https://github.com/jonathanhaslett/PrivacyPi.

##Getting started with the Raspberry Pi and Raspbian

###Requirements:

A Raspberry Pi kit including:
* Raspberry Pi. I used a [Raspberry Pi 2 Model B](https://www.raspberrypi.org/products/raspberry-pi-2-model-b/)
* 8GB MicroSD memory card
* Micro USB cable
* USB charger capable of [powering the Pi](https://google.com/)
* Ethernet cable
* A housing to keep your Pi safe and secure after installation. I went with [this](https://google.com/).

Along with these extras for getting set up:
* Monitor with HDMI input
* HDMI cable
* USB keyboard
* USB mouse

###Install Raspbian using NOOBS

NOOBS stands for **New Out Of the Box Software** and is the installer and recovery platform that we'll use to install Raspbian on our Pi.

Follow the [NOOBS Setup Guide](https://www.raspberrypi.org/help/noobs-setup/) and you'll end up on the desktop of a fresh installation of Raspbian and ready to continue building your PrivacyPi.

###Change some settings

Once you're on the Raspbian desktop head over to the settings and update these settings:

* Change your devices name to PrivacyPi
* Update your date, time, and location
* Set your language and keyboard layout
* Set your Pi to start in the command line interface (CLI), this will free up resources when you no longer need the desktop. The command to manually load the desktop whenever you need to is ```startx```

For some added local security you could go ahead and change the password for the ```pi``` user to something other than the default ```raspberry```. This could be helpful if you regularly have people poking around on your local network.

###Configure your local network

It's important that your Pi is always assigned the same IP address by your router (DHCP Server). The good news is that most routers these days make it easy for you to manually reserve an IP address for your device. 

Login to your router's management interface and reserver an IP address for your Pi that matches your existing network, usually this will look like ```192.168.0.x``` or ```192.168.1.x```. Refer to your routers documentation for specific instructions but usually you'll need to find the table showing all the connected devices (DHCP Table) and choosing a device to reserve an address for. You'll recall we renamed the Pi to ```PrivacyPi``` so it should be easy to identify from the table.

For the rest of the tutorial we're going to assume you've assigned the IP address ```192.168.1.251``` to your Pi which has the hostname ```PirvacyPi```.

Restart your Pi. It should load to the CLI. 

Once at the CLI you can confirm that you've been assigned the correct IP address by typing the command:

	ifconfig -a

Hopefully you should see an output that starts with something like:

	eth0 Link encap:Ethernet  HWaddr XX:XX:XX:XX:XX:XX  
    inet addr:192.168.1.251  Bcast:192.168.1.255  Mask:255.255.255.0
    inet6 addr: fe80::ba27:ebff:fe54:bee5/64 Scope:Link
    UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
    RX packets:607649 errors:0 dropped:1097 overruns:0 frame:0
    TX packets:492564 errors:0 dropped:0 overruns:0 carrier:0
    collisions:0 txqueuelen:1000    
    
##Install PrivacyPi Bundle

This step could technically be completed using the Pi's CLI but for a lot of users it will be easier to load up the desktop by typing ```startx```.

1. Open the Pi's web browser and head to [privacypi.io](http://privacypi.io/) and ownload the [latest PrivacyPie zip archive](https://github.com/jonathanhaslett/PrivacyPi/zipball/master).
2. Extract the files from the zip archive.
3. Opens the Pi's file manager program and move all the files you just extracted to your home directory, if you're still using the default "pi" user then you're home directory will be located at ```/home/pi/```.
4. Open the Pi's terminal client. It should automatically place you in your home directory, shown as ```~```. If you enter the command ```ls``` it should show a listing of the home directory folders along with the files you just moved during the previous step.
5. Run the PrivacyPi install script by typing the command ```sh install```. The script will use ```apt-get``` to install the software needed and make a few changes so that you're ready to move on to the next steps below.

##OpenVPN - VPN Client

Using the Pi's web browser, download the OpenVPN configuration files from your VPN provider's website. The file with either be a single ```*.conf``` file or an archive containing multiple files such as a ```*.conf``` files and some certificate files.

Edit the main .conf file with the Pi's text editor and add these two lines t the bottom of the file:

	script-security 2
	up /home/pi/vpn-up
	down /home/pi/vpn-down
	
These commands will automatically start and stop the proxy servers when the VPN starts or stops. This prevents a problem that Gavin [described](http://www.privacypi.org/making-things-a-little-more-private/) where you'll run into problem if a proxy server tries to start using the VPN's ```tun0``` network interface before it exists.

The next step is to move the files but you'll need Super User privileges to write to ```/etc/openvpn/``` so close the file manager and then launch it from the terminal with SU privileges by typing ```sudo pcmanfm```.

Move all the files provided by your VPN provider to the ```/etc/openvpn/```` directory. When OpenVPN start's it will search this folder for a *.conf file and use those details to connect to your VPN.

##How to start OpenVPN Automatically When the RPi Starts

Now that the OpenVPN server is installed, it is time to make it run as a service every time the Pi boots up.

Edit the file ```/etc/default/openvpn``` with:

	sudo pico /etc/default/openvpn

Uncomment the line ```#AUTOSTART="all"``` and either leave it as all, or replace it with the name of the specific server ```*.conf``` file you'd like to automatically connect.

That's it, OpenVPN should be ready to go unless there's a problem with with your provided config. We'll get around to testing everything in a minute.

##Privoxy - HTTP/HTTPS Proxy

*Privoxy is a non-caching web proxy with advanced filtering capabilities for enhancing privacy, modifying web page data and HTTP headers, controlling access, and removing ads and other obnoxious Internet junk.* - [privoxy.org](http://www.privoxy.org/)

Only minor changes are required for privoxy.

Open the config file by typing:

	sudo pico /etc/privoxy/config
	
Search for the variable named ```listen-address``` and change it's value to your Pi's IP address, for example I'm using ```192.168.1.251```.

##Dante - SOCKS5 Proxy

Dante is a [SOCKS proxy](https://en.wikipedia.org/wiki/SOCKS) that we'll use to route non-web traffic through the VPN connection. 

The ```sh install``` script has done most the heavy lifting here. we've replaced Dante's config file that tells Dante to accept incoming connections on port ```1080``` and route traffic through the VPN connection labeled ```tun0```.

If, in the future you want to make further config changes, you can view and edit the Dante config file by typing:

	sudo pico /etc/danted.conf

##Testing your new software.

If you extracted all the files to your ```/home/pi``` home directory you'll be able to type these commands to control the software we've just installed:

```sh ~/start``` - start OpenVPN, and thanks to your config file changes start Privoxy/Dante once the VPN is connected.

```sh ~/stop``` - stop OpenVPN, and thanks to your config file changes stop Privoxy/Dante as well.

```sh ~/status``` - output the status for the VPN connection (tun0) and well as the status of the services OpenVPN, Privoxy, and Dante.

When OpenVPN has connected and Privoxy+Dante have successfully started, the ```sh ~/status``` command should output something similar to this:

	output of status command
	output of status command
	output of status command
	
If it looks like your VPN ```tun0``` is connected and Privoxy+Dante are running you should be ready to open up your PC's browser/torrent software and enter the proxy settings.

For a HTTP/HTTPS proxy you'll need to enter the Pi's IP Address plus port ```8118```.

For a SOCKS5 proxy you'll need to enter the Pi's IP Address plus port ```1080```.

After setting up the proxy in your web browser, test that it's working by visiting the website https://ipleak.net/ to confirm that your network traffic is being routed through the VPN.
