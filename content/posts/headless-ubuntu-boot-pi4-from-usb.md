---
title: "Running Ubuntu Headless on Pi4 from USB"
date: 2021-11-18T19:14:57+02:00
slug: "raspberry-pi4-headless-ubuntu-from-usb"
category: 
summary: Run Ubuntu headless on Raspberry Pi 4 while booting from your USB without using an external monitor or keyboard, all through your host computer.  
description: Running a headless OS on your Raspberry Pi can get quite frustrating if you don't want to buy unnecessary accessories for connecting your Pi to a screen and keyboard. With the Pi 4, you can directly use your USB to boot the OS of your choice.
cover:
  image: /img/headless-ubuntu-boot-pi4-from-usb/raspberrypi-server.jpg
  alt: Raspberry Pi Running Ubuntu Server
  caption: Turn your Raspberry Pi into a server!
  relative: false
showtoc: true
draft: false
---

### Can you boot from a USB drive?

If you have been a Raspberry Pi enthusiast then you must have faced this dilemma, whether to keep using the SD card or to use a USB drive to boot the OS of your choice. There is a workaround for the older versions. But with Raspberry Pi 4 you can install the OS on your USB, plug it into your Pi and you are good to go!

For those of you that are not long term enthusiasts and have just got hold of a brand new Raspberry Pi 4, here is a short and sweet guide on setting up your Raspberry Pi and getting it up and  running into the Pi-verse.

### Things you will need

- <a href= "https://www.raspberrypi.com/products/raspberry-pi-4-model-b/" style = "color : #e30b5d;" target = "_blank"><strong>Raspberry Pi</strong></a>
- USB drive (I am using a <a href= "https://www.transcend-info.com/Products/No-422" style = "color : Teal;" target = "_blank"><strong>USB 3.1, 32 GB</strong></a> USB drive for booting)
- <a href= "https://www.raspberrypi.com/products/type-c-power-supply/" style = "color : #e30b5d;" target = "_blank"><strong>Raspberry Pi Power Supply</strong></a>
- <a href= "https://www.spectrum.net/support/internet/what-ethernet-cable/" style = "color : Teal;" target = "_blank"><strong>Ethernet cable</strong></a>
- <b>Your computer from where you will access your Pi</b>
 

### Preparing your Bootable USB drive

You can use various softwares out there for this step. But for me only the <a href= "https://www.raspberrypi.com/software/" style = "color : #e30b5d;" target = "_blank"><strong>Raspberry Pi Imager</strong></a> worked. It is provided by the official Raspberry Pi website, so you can rest assured that it is specifically made for your needs.

For Windows, download it from <a href= "https://downloads.raspberrypi.org/imager/imager_latest.exe" style = "color : #e30b5d;" target = "_blank"><strong>this link</strong></a>. Run the downloaded `.exe` file and install the imager on your machine. If you are using Linux you can type `sudo apt install rpi-imager` into your terminal to install it.

Although this post talks about running the Ubuntu Server on your raspberry, you can install any OS of your choice as long as the hardware requirements of the OS are met by your Pi. Where there is a will, there is a way 😉.

You can follow the Pi Imager's guidelines to install Ubuntu Server but it did not work for me somehow, so I will suggest that you download and install it from the <a href= "https://ubuntu.com/download/raspberry-pi" style = "color : #e30b5d;" target = "_blank"><strong>source</strong></a> manually. If you have a 4 GB RAM on your Pi 4 then you can safely choose the <a href= "https://ubuntu.com/download/raspberry-pi/thank-you?version=21.10&architecture=server-arm64+raspi" style = "color : #e30b5d;" target = "_blank"><strong>64-bit version</strong></a> or if you want to play it safe then go for the <a href= "https://ubuntu.com/download/raspberry-pi/thank-you?version=21.10&architecture=server-armhf+raspi" style = "color : #e30b5d;" target = "_blank"><strong>32-bit version</strong></a>. 

*I am using the latest release of the Ubuntu Server, the LTS version straight up refused to boot on my Pi.*

Now simply follow these steps:

1. Plug in the USB into your PC.
2. Open Pi Imager.
3. Click the the OS option as shown in **Fig. 1** below. 
4. Scroll down when the prompt appears as shown in **Fig.2**. Choose the **"Use custom"** option. You will be prompted to choose a file from your computer. Choose the downloaded Ubuntu `.XZ` file that you downloaded earlier.
5. Click the Storage option as shown in **Fig. 1** and choose the plugged in USB from the options available.
6. Click **"write"** as shown in **Fig. 3**  and wait for the USB to be written over and verified. If there is an error during verification, try to be a bit patient and write the file again. 
7. Eject your USB when prompted.

![Raspberry Pi Imager options](/img/headless-ubuntu-boot-pi4-from-usb/raspberry-pi-imager1.jpg)
![Raspberry Pi Imager Choosing the USB drive](/img/headless-ubuntu-boot-pi4-from-usb/raspberry-pi-imager2.jpg)

### Finding the IP address of your Pi 

You will require the IP address of your Pi to access it via SSH. Again, you can use many ways to achieve that such as logging into your **router** and finding the list of devices, but I will illustrate the method through <a href= "https://nmap.org/" style = "color : teal;" target = "_blank"><strong>Nmap</strong></a>. To use it first you will need to install it. 

1. If you are on Windows, go to the <a href= "https://nmap.org/download.html" style = "color : teal;" target = "_blank"><strong>download page of Nmap</strong></a> and download the binary package for your Windows version. If you are on a Linux distribution, run the command `sudo apt-get install nmap` to install it.
2. Install it and choose the default settings. If you are a Super-User you can tweak it to your liking 👨‍💻.
3. Find out the IP address of your machine first by typing `ipconfig` for Windows and `ifconfig` for Linux. If that command does not work on Linux then install net-tools first through `sudo apt install net-tools`. 
4. Your IP address is divided into 4 parts, for example something like `129.789.9.5`. Now what you have to do is to remove the last part and instead of it insert `0/24`. Hence, `129.789.9.5` becomes `129.789.9.0/24`.
5. Plug in the bootable USB and the ethernet cable into your Pi and power it on. The red and green signals will light up and the green one will start blinking like a heartbeat 💓 if the boot was successful.   
6. Enter the Nmap command in the terminal : `nmap -sP <edited IP address>`, for example: `nmap -sP 129.789.9.0/24`.

You will get a list of the devices connected to your wifi on your terminal. The Raspberry Pi's IP address is probably under the name `Raspberry Pi Trading`. Copy it, you will require it in the next step.

### SSH-ing into your Pi

Again you can use various softwares that allow you to SSH into a device remotely, but like Linux, you can now do it from Windows Terminal as well. If you have not yet heard of or used Windows Terminal, here is a good <a href= "https://bit.ly/3paAg5t" style = "color : teal;" target = "_blank"><strong>reason</strong></a> to set it up.

In the Windows Terminal simply type `ssh ubuntu@<The IP address of the Pi>` and you will receive a message something like:

> The authenticity of host 'Raspberry Pi's IP address (IP)' can't be established.

Type `yes`. 

The connection will be closed and you will have to SSH into the Pi again. The default password is `ubuntu`. You will be required to change it immediately and the connection will be closed again. After this step you can safely SSH into your Pi and now it is completely yours to fiddle with! 

PS: Here is my Raspberry Pi 4. I am using an armour case which keeps the CPU temperature around 32°C-36°C. You can even keep it on a laptop cooler to achieve some cooling. 

![Raspberry Pi 4 with Armour case for extra cooling](/img/headless-ubuntu-boot-pi4-from-usb/raspberrypi4.jpg)
