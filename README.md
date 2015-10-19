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

Hopefully you should see an output that looks like:

	eth0 sdjfkjsakdjfhkjsadhfkjhajksdhf'
	sdfgfdgsdfg
	



##OpenVPN - VPN Client

##Privoxy - HTTP/HTTPS Proxy

##Dante - SOCKS5 Proxy

##Customise the startup order

##Testing