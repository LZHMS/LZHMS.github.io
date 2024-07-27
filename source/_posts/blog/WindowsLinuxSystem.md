---
title: Install the Operating System of Windows and Linux on the Laptop
date: 2024-07-08 21:20:40
toc: true
tags:
    - Windows
    - Linux
categories: blog
cover: https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.5c0ux5gfzo.webp
excerpt: This is a post about how to simultaneously install the operating system of Windows and Linux on the laptop.
---

## Environments
+ Windows 11
+ Ubuntu 24.04 LTS
+ Laptop: Yoga Pro 14S ARH7
+ Allocated Disk For Ubuntu: 146.5GB

## Step I: Download Ubuntu Package
+ Download the Ubuntu ISO file from the official website: https://ubuntu.com/download/desktop

+ Just select the version of `Ubuntu 24.04 LTS` and download the ISO file.
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.4qr79dpl0u.webp" alt="" width='70%'/>

## Step II: Make a Bootable USB Stick
### Preparation
+ Prepare a USB stick with a capacity of at least `8GB`
The USB stick will be formatted so you need to back up any important data on it.
+ Install the software _UltraISO_
We use the software _UltralSO_ to write the ISO file to the USB stick. _UltraISO_ requires a paid registration, but we only need to use it for a short period, so we can click 'Trial' without purchasing a registration code.

### Steps
+ Open the _UltraISO_ and select the downloaded ISO file from the local directory
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/docs/xdu/202407061336764.png" alt="" width='70%'/>

+ Double-click the ISO file: `ubuntu-24.04-desktop-and64.iso`
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/docs/xdu/202407061337629.png" alt="" width='70%'/>

+ Select "Boot" in the menu bar above _UltraISO_, then click on "Write Disk Image"
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/docs/xdu/202407061337990.png" alt="" width='70%'/>

+ Check the configuration
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/docs/xdu/202407061339055.png" alt="" width='70%'/>

+ Format the USB stick by clicking the button of 'format'
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/docs/xdu/202407061339980.png" alt="" width='70%'/>

+ Write the ISO file to the USB stick by clicking the button of 'Write'
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/docs/xdu/202407061428710.png" alt="" width='70%'/>

This process will take a long time period and after writing the stage have just been finished.

## Step III: Allocate the Disk Space for Ubuntu
+ Open the disk management tool in Windows
You can search the `Computer Management` in the search bar and open it. And in the left menu, select `Disk Management` under the `Memory` section.


+ Right-click on the disk, preferably `D` Disk, that you want to allocate space for Ubuntu and select `Shrink Volume`
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.1sex79rhpq.webp" alt="Shrink Volume For Ubuntu" width='70%'/>

+ Enter the amount of space you want to allocate for Ubuntu, and then click "Shrink"
It will be recommended to allocate around `100GB` of space for Ubuntu. This will make sure that you have enough space for the operating system and other applications.

## Step IV: Install Ubuntu
+ Insert the USB stick into the computer

### Method I: BIOS mode
Press the key of `F12` or `F2` to enter the BIOS setting and select the USB stick as the boot device

### Method II: Advanced Restart
Sometimes entering the BIOS setting is not easy, but there have another way to boot the system from the USB stick.
+ Search the `Restore` in the windows search bar and open it.
+ Click on the `Restart Immediately` button to restart the computer.
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/docs/xdu/20240706173041.png" alt="Restart the System" width='70%'/>

+ After the computer restarts, it will enter the Advanced Boot Options menu. Select the `Using Device` option.
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/微信图片_20240708104657.51e13y0dg4.webp" alt="Select the Boot Options" width='70%'/>

+ About `EFI` mode we just choose the `EFI USB Device` to boot the operating system.
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/微信图片_20240708104716.1759kzm6r7.webp" alt="EFI USB Device" width='70%'/>

+ Entering this page, we just choose to install Ubuntu
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/微信图片_20240708105456.4qr7asv5mr.webp" alt="Install Ubuntu" width='70%'/>

+ When it boots the Ubuntu system, it will prepare the environment.

<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/微信图片_20240708105502.39l291soeu.webp" alt="Prepare Ubuntu" width='70%'/>

+ Configure the Ubuntu system and just wait for several minutes.
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/微信图片_20240708105506.9rja2d5g9r.webp" alt="Configure Ubuntu" width='70%'/>

+ When all installation have been finished
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/微信图片_20240708105510.6m3s3fcsyf.webp" alt="Installation Finished" width='70%'/>

## Step V: Restart the System
After the installation, we need to restart the system to make sure that the system is working properly.

<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/微信图片_20240708114917.ese3b9nk9.webp" alt="Boot Two Systems" width='70%'/>

+ Boot Ubuntu 
We just click the `Ubuntu` button to boot the Ubuntu system.
+ Boot Windows
We just click the `Windows Boot Manager` button to boot the Windows system.

## Configuration
We can see that the system is installed successfully, but we still need to configure it. For personalized usage needs, we can configure specific applications. Firstly, just have a look for my configuration.
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.5c0ux5gfzo.webp" alt="My Configuration" width='70%'/>

### System Fonts
+ Install the packages

``` bash
sudo apt install gnome-tweaks
```
+ Run the command

```bash
gnome-tweaks
```
It will open the configuration interface, and we just click `Fonts`. It has three fonts to configure the displayed fonts style under different positions. And importantly, it has the `Scaling Factor` below the interface to configure the interface size.
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.92q0id0dnj.webp" alt="Configuration Interface" width='70%'/>
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.3nrhzxpr49.webp" alt="Fonts Configuration" width='70%'/>


### Chinese Input
+ Install the packages
There has several Chinese input methods, but I recommend `ibus-libpinyin` because it is simple and easy to use.

```bash
sudo apt-get install ibus-libpinyin
```
+ Run the command
After installation, we need to `log out` to fresh the package. And logging in again, we just run the following command to add the `Pinyin` method under the `Chinese` section.

```bash
ibus-setup
```
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.1759l0pdu7.webp" alt="Install Pinyin" width='70%'/>

+ Configure the input method in the system settings
Just see the following picture. We need to add the input source of `Chinese(Pinyin)` under the `Keyboard` section.

<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.1759l0uf37.webp" alt="Chinese Input" width='70%'/>

### Clash Verge
+ Download the package from website: https://github.com/clash-verge-rev/clash-verge-rev/releases

+ Follow the instructions to install the package: https://clash-verge-rev.github.io/faq/linux.html
+ A Fantastic Application

<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.4uat8jx2xa.webp" alt="Clash Verge" width='70%'/>

### Blue Mail
It's also a fantastic application on linux system. It is a mail client and it has a very beautiful interface.

<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/image.3yebt3qt7n.webp" alt="Blue Mail" width='70%'/>

### Wemeet
+ Download the package from website: https://meeting.tencent.com/download/
+ Install the package and run it.
+ Solve the problem: `wayland`
```bash
sudo vim /etc/gdm3/custom.conf
#WaylandEnable=false   ==> WaylandEnable=false
sudo service gdm3 restart
```

## Installation Command
### `.deb` file
```bash
sudo dpkg -i package_file.deb
sudo apt install ./package_file.deb
```
### `.AppImage` file
```bash
chmod +x package_file.AppImage
./package_file.AppImage
```
### `.rpm` file
```bash
sudo rpm -i package_file.rpm
```