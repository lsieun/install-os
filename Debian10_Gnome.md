# Debian 10 Gnome

<!-- TOC -->

- [Debian 10 Gnome](#debian-10-gnome)
  - [1. Wayland](#1-wayland)
  - [2. Display Managers](#2-display-managers)
    - [2.1. GDM3](#21-gdm3)
    - [2.2. SDDM](#22-sddm)
    - [2.3. LightDM](#23-lightdm)
  - [3. Wayland and Nvidia](#3-wayland-and-nvidia)
  - [4. Display Manager](#4-display-manager)
    - [4.1. check display manager](#41-check-display-manager)
  - [5. Reference](#5-reference)

<!-- /TOC -->

如何知道使用Wayland还是X11：

```bash
echo $XDG_SESSION_TYPE
```

登陆的时候选择GNome On Xorg，可能会避免一些不必要的问题。

## 1. Wayland

Wayland is a communication protocol that specifies the communication between a display server and its clients. A display server using the Wayland protocol is called a Wayland compositor, because it additionally performs the task of a compositing window manager.

The aim of **Wayland** is replacing the **X Window System** (Also known as **X11**, or **Xorg**) with a modern, secure, and simpler windowing system. As part of its efforts, the Wayland project also develops a reference implementation of a Wayland compositor called Weston. Major desktops such as KDE Plasma and GNOME have implemented their own Wayland compositors.

## 2. Display Managers

### 2.1. GDM3

**GDM** (**GNOME Display Manager**) will automatically use **Wayland** when supported, except when using the proprietary **NVIDIA** driver, in which case it will fall back to X11 due to instability.

To use the X11 backend by default, uncomment the `WaylandEnable=false` line in the `/etc/gdm3/daemon.conf` file.

### 2.2. SDDM

Wayland support for **SDDM**(**Simple Desktop Display Manager**) itself is in progress, currently it uses **X11** by default everywhere. SDDM is still capable of starting a Wayland session for a desktop however.

### 2.3. LightDM

There is little hope of LightDM obtaining native Wayland support, however it can still start a Wayland session for a desktop.

## 3. Wayland and Nvidia

Wayland not work if you use the Nvidia property driver .. use the free drivers and you can see the wayland session.

## 4. Display Manager

The display manager runs before you log in; it's chosen by the system administrator. Everything else (window manager, session manager, desktop environment, …) is chosen by the user.

### 4.1. check display manager

Unfortunately the configuration differs for each distribution:

Debian/Ubuntu

```text
$ cat /etc/X11/default-display-manager
```

RedHat (should also apply to Fedora)

```text
cat /etc/sysconfig/desktop
```

OpenSuSe

```text
cat /etc/sysconfig/displaymanager
```

If you're using `systemd`, then

```bash
systemctl status display-manager
```

Will display the name and status of the active display manager service on your machine.

## 5. Reference

- [Wayland](https://wiki.debian.org/Wayland)
