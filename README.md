# PrivacyPi
Tutorial for turning a Raspberry Pi into an Internet data retention privacy solution using Raspbian, OpenVPN, Privoxy, and Dante.

Inspired by Gavin's work at http://www.privacypi.org/ I've created this project to   

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

It's important that your PrivacyPi always

##OpenVPN - VPN Client

##Privoxy - HTTP/HTTPS Proxy

##Dante - SOCKS5 Proxy

##Customise the startup order

##Testing