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

* Device Name - Change to PrivacyPi
* Localisation - Set your time, date, and keyboard layout, useful when working with config files later on
* Start in cli - use startx to launch gui
* 

###Configure your local network

It's important that your Pi is always assigned the same IP address by your router (DHCP Server). The good news is that most routers these days make it easy for you to manually reserve an IP address for your device. 

Login to your router's management interface and reserver an IP address for your Pi that matches your existing network, usually this will look like ```192.168.0.x``` or ```192.168.1.x```. Refer to your routers documentation for specific instructions but usually you'll need to find the table showing all the connected devices (DHCP Table) and choosing a device to reserve an address for. You'll recall we renamed the Pi to ```PrivacyPi``` so it should be easy to identify from the table.

For the rest of the tutorial we're going to assume you've assigned the IP address ```192.168.1.251``` to your Pi which has the hostname ```PirvacyPi```.

Restart your Pi. It should load to the command line interface *(CLI)*. 

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

https://github.com/jonathanhaslett/PrivacyPi/zipball/master

##OpenVPN - VPN Client

##Privoxy - HTTP/HTTPS Proxy

*Privoxy is a non-caching web proxy with advanced filtering capabilities for enhancing privacy, modifying web page data and HTTP headers, controlling access, and removing ads and other obnoxious Internet junk.* - [privoxy.org](http://www.privoxy.org/)

Only minor changes are required for privoxy.

Open the config file by typing:

	sudo pico /etc/privoxy/config
	
Search for the variable named ```listen-address``` and change it's value to your Pi's IP address, we're using ```192.168.1.251```.

##Dante - SOCKS5 Proxy

Dante is a [SOCKS proxy](https://en.wikipedia.org/wiki/SOCKS) that we'll use to route non-web traffic through the VPN connection. 

The ```sh install``` script has done most the heavy lifting here. we've replaced Dante's config file that tells Dante to accept incoming connections on port ```1080``` and route traffic through the VPN connection labeled ```tun0```.

If you want to make further config changes, you can view and edit the Dante config file by typing:

	sudo pico /etc/danted.conf

##Some basic shell scripts to make life easier

If you extracted all the files to your /home/pi home directory you'll be able to type these commands to control your PrivacyPi:

```sh ~/start``` - start OpenVPN, and thanks to your config file changes start Privoxy/Dante once the VPN is connected.

```sh ~/stop``` - stop OpenVPN, and thanks to your config file changes stop Privoxy/Dante as well.

```sh ~/status``` - output the status for the VPN connection (tun0) and well as the status of the services OpenVPN, Privoxy, and Dante.

##Testing