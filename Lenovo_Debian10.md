# Lenovo Debian 10

<!-- TOC -->

- [Lenovo Debian 10](#lenovo-debian-10)
  - [1. Download Debian 10 ISO](#1-download-debian-10-iso)
  - [2. Install Debian](#2-install-debian)
  - [3. After Installation](#3-after-installation)
    - [3.1. add sudoer](#31-add-sudoer)
    - [3.2. Debian Package Mirror](#32-debian-package-mirror)
    - [3.3. update and upgrade](#33-update-and-upgrade)
    - [3.4. solve iwlwifi problems](#34-solve-iwlwifi-problems)
    - [3.5. Install latest AMD Drivers](#35-install-latest-amd-drivers)
  - [4. Settings](#4-settings)
    - [4.1. Fix Date and time](#41-fix-date-and-time)
  - [5. sudo apt install](#5-sudo-apt-install)
    - [5.1. fcitx(Free Chinese Input Toy of X)](#51-fcitxfree-chinese-input-toy-of-x)
    - [5.2. vim](#52-vim)
    - [5.3. git](#53-git)
    - [5.4. gnome-tweak-tool](#54-gnome-tweak-tool)
    - [5.5. video player](#55-video-player)
    - [5.6. video recorder](#56-video-recorder)
    - [5.7. gnome-paint](#57-gnome-paint)
    - [5.8. curl](#58-curl)
    - [5.9. chromium](#59-chromium)
    - [5.10. Install Wireshark](#510-install-wireshark)
    - [5.11. SSH Server](#511-ssh-server)
    - [5.12. screen shot](#512-screen-shot)
    - [5.13. Filezilla](#513-filezilla)
    - [5.14. net-tools](#514-net-tools)
  - [6. uninstall software](#6-uninstall-software)
  - [7. debian package](#7-debian-package)
    - [7.1. Visual Studio Code](#71-visual-studio-code)
    - [7.2. Install JDK](#72-install-jdk)
    - [7.3. VeraCrypt](#73-veracrypt)
    - [7.4. Install Virtualization Software (VirtualBox)](#74-install-virtualization-software-virtualbox)

<!-- /TOC -->

## 1. Download Debian 10 ISO

下载页面：`http://mirror.bit.edu.cn/debian-cd/10.4.0-live/amd64/iso-hybrid/`

ISO文件名：`debian-live-10.4.0-amd64-gnome.iso`

文件下载地址：

```text
http://mirror.bit.edu.cn/debian-cd/10.4.0-live/amd64/iso-hybrid/debian-live-10.4.0-amd64-gnome.iso
```

## 2. Install Debian

在安装的过程中，用网线连接笔记本。

如果遇到`iwlwifi-8265-*.ucode`不存在的情况，就先跳过（选择`No`）；等到安装完成后，再处理`iwlwifi`的问题。

## 3. After Installation

### 3.1. add sudoer

切换到root用户

```bash
$ su root
```

将tom用户添加到sudoer中

```bash
# sudo usermod -aG sudo tom
```

上一步操作之后，可能需要重启或重新登录。

Use the `sudo` command to run the `whoami` command:

```bash
$ sudo whoami
```

If the user has **sudo** access then the output of the `whoami` command will be `root`.

### 3.2. Debian Package Mirror

清华大学Debian软件源镜像地址

```text
https://mirrors.tuna.tsinghua.edu.cn/help/debian/
```

修改以下文件：

```bash
/etc/apt/sources.list
```

修改为以下内容：

```text
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ buster main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian-security buster/updates main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security buster/updates main contrib non-free
```

如果遇到无法拉取`https`源的情况，请先使用`http`源并安装：

```bash
$ sudo apt install apt-transport-https ca-certificates
```

再使用软件源镜像。

### 3.3. update and upgrade

```bash
$ sudo apt update
$ sudo apt upgrade
```

### 3.4. solve iwlwifi problems

这里是解决wifi不能上网的问题。

首先，安装firmware-iwlwifi

```bash
$ sudo apt install firmware-iwlwifi
```

```bash
$ sudo rfkill list all
$ sudo modprobe -r ideapad_laptop
```

以上命令需要在每次开机后都运行一次，非常麻烦。为了下次开机wifi可以自动使用，使用命令

```bash
cd /etc/modprobe.d
$ sudo vi blacklist.conf
```

如果`blacklist.conf`不存在，那就新建一个，例如`touch blacklist.conf`。

打开文件，并在最后一行加入：

```text
blacklist ideapad_laptop
```

至此配置wifi结束。重启电脑可以自动搜到wifi热点了。

### 3.5. Install latest AMD Drivers

You can also install additional AMD drivers needed like the graphics card, ATI Proprietary and Nvidia Graphics drivers. To Install the latest AMD Drivers, first we must modify `/etc/apt/sources.list` file, add `non-free` word in lines which contains `main` and `contrib`, example is shown below

```bash
pkumar@linuxtechi:~$ sudo vi /etc/apt/sources.list
…………………
deb http://deb.debian.org/debian/ buster main non-free contrib
deb-src http://deb.debian.org/debian/ buster main non-free contrib

deb http://security.debian.org/debian-security buster/updates main contrib non-free
deb-src http://security.debian.org/debian-security buster/updates main contrib non-free

deb http://ftp.us.debian.org/debian/ buster-updates main contrib non-free
……………………
```

Now use the following apt commands to install latest AMD drivers in Debian 10 system

```bash
pkumar@linuxtechi:~$ sudo apt update
pkumar@linuxtechi:~$ sudo apt install firmware-linux firmware-linux-nonfree libdrm-amdgpu1 xserver-xorg-video-amdgpu -y
```


## 4. Settings

### 4.1. Fix Date and time

Go to System **Settings** –> **Details** –> **Date and Time** and then change your time zone that suits to your location.

## 5. sudo apt install

### 5.1. fcitx(Free Chinese Input Toy of X)

安装五笔输入法：

```bash
# 删除ibus
sudo apt purge ibus

sudo apt install im-config
sudo apt install fcitx
sudo apt-get install fcitx-config-gtk
sudo apt-get install fcitx-table-wbpy
sudo apt-get install fcitx-ui-light
```

安装完后，需要重启系统，然后在终端中输入

```bash
$ fcitx-config-gtk3
```

### 5.2. vim

```bash
$ sudo apt install vim
```

查看版本

```bash
$ vim --version
```

### 5.3. git

```bash
$ sudo apt install git
$ git config --global user.name "lsieun"
$ git config --global user.email "515882294@qq.com"
$ git config --global core.autocrlf input
$ git config --global core.editor vim
```

查看版本

```bash
$ git --version
```

### 5.4. gnome-tweak-tool

安装桌面管理工具：

```bash
$ sudo apt install gnome-tweak-tool
```

### 5.5. video player

安装视频播放器：

```bash
sudo apt install mpv
```

### 5.6. video recorder

安装视频录制软件：

```bash
sudo apt install kazam
```

### 5.7. gnome-paint

安装画图工具

```bash
sudo apt install gnome-paint
```

### 5.8. curl

```bash
$ sudo apt install curl
```

### 5.9. chromium

安装Chromium

```bash
$ sudo apt install chromium
```

运行Chromium

```bash
$ chromium
```

### 5.10. Install Wireshark

```bash
sudo apt -y install wireshark
```

启动之后，遇到一个错误：

```text
couldn't run /usr/bin/dumpcap in child process: permission denied
```

Open a terminal and type the following commands:

```bash
sudo dpkg-reconfigure wireshark-common
```

press the right arrow and enter for `yes`

```bash
sudo chmod +x /usr/bin/dumpcap
```

you should now be able to run it without root and you will be able to capture.

### 5.11. SSH Server

```bash
sudo apt install openssh-server
```

On Debian, the default behavior of OpenSSH server is that it will start automatically as soon as it is installed. It is also listening on port 22. You can also check whether OpenSSH server is running on it with the following command:

```bash
sudo systemctl status ssh
```

If in any case OpenSSH server is not running, you can run the following command to start OpenSSH server.

```bash
sudo systemctl start ssh
```

To start OpenSSH server on boot

```bash
sudo systemctl enable ssh
```

To disable OpenSSH server from startup:

```bash
sudo systemctl disable ssh
```

### 5.12. screen shot

在debian下有一个很方便的截图工具`gnome-screenshot`，在命令行模式下输入`gnome-screenshot -a`可自定义截图区域，默认保存在用户跟目录。其他的选项输入`man gnome-screenshot`查看更多选项。

```bash
$ sudo apt install flameshot -y
```

- `flameshot` just idles immediately.
- `flameshot gui` exists immediately (exit code 0).
- `flameshot config` exists immediately (exit code 0).

截图软件 shutter

```bash
sudo apt install shutter
```

注意： Shutter 工具在 Debian 10 中已被移除。

### 5.13. Filezilla

FileZilla - The free FTP solution

To install Filezilla in your system use the following `apt` command

```bash
sudo apt install filezilla -y
```

### 5.14. net-tools

```bash
sudo apt install net-tools
```

This includes `arp`, `ifconfig`, `netstat`, `rarp`, `nameif` and `route`.

## 6. uninstall software

- [ ] Debian 10自带了许多游戏，删除它们

## 7. debian package

### 7.1. Visual Studio Code

下载地址：

```bash
https://code.visualstudio.com/
```

安装

```bash
$ sudo dpkg -i code_*.deb
```

### 7.2. Install JDK

```bash
$ sudo tar -zxvf jdk-8u241-linux-x64.tar.gz -C /usr/local/
$ cd /usr/local/
$ sudo ln -s jdk1.8.0_241/ jdk8
```

配置jdk环境变量

```bash
sudo vim /etc/profile
```

在文件结尾添加如下配置：

```text
export JAVA_HOME=/usr/local/jdk8
export PATH=$PATH:${JAVA_HOME}/bin
```

通过如下命令让`/etc/profile`的配置立即生效

```bash
$ source /etc/profile
```

测试JDK的版本

```bash
$ java -version
java version "1.8.0_241"
Java(TM) SE Runtime Environment (build 1.8.0_241-b07)
Java HotSpot(TM) 64-Bit Server VM (build 25.241-b07, mixed mode)
```

### 7.3. VeraCrypt

下载地址：

```text
https://software.opensuse.org/package/veracrypt
```

安装

```bash
sudo dpkg -i veracrypt_1.23_ds1-0.1_obs_amd64.deb
```

或者

```bash
echo 'deb http://download.opensuse.org/repositories/home:/unit193:/veracrypt/Debian_10/ /' | sudo tee /etc/apt/sources.list.d/home:unit193:veracrypt.list
curl -fsSL https://download.opensuse.org/repositories/home:unit193:veracrypt/Debian_10/Release.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/home:unit193:veracrypt.gpg > /dev/null
sudo apt update
sudo apt install veracrypt
```

### 7.4. Install Virtualization Software (VirtualBox)

First step in installing Virtualbox is by importing the public keys of the Oracle VirtualBox repository to your Debian 10 system

```bash
pkumar@linuxtechi:~$ wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
OK
pkumar@linuxtechi:~$ wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -
OK
pkumar@linuxtechi:~$
```

If the import is successful, you will see a “OK” message displayed.

Next you need to add the repository to the source list

```bash
pkumar@linuxtechi:~$ sudo add-apt-repository "deb http://download.virtualbox.org/virtualbox/debian buster contrib"
pkumar@linuxtechi:~$
```

Finally, it is time to install VirtualBox 6.0 in your system

```bash
pkumar@linuxtechi:~$ sudo apt update
pkumar@linuxtechi:~$ sudo apt install virtualbox-6.0 -y
```




