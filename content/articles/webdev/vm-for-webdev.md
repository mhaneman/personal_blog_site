+++ 
draft = false 
date = 2024-11-01
title = "Setup a Minimal Web Development Virtual Machine in QEMU for Arch Linux Hosts"
description = ""
slug = ""
authors = ["Michael Haneman"]
tags = ["Hugo", "Golang", "HTML", "SCSS"]
categories = ["Web Dev"]
externalLink = ""
series = []
+++

The following technology is going to be utilized:

1. QEMU
2. Virt manager
3. Debian
4. i3
5. vscode
6. Node Version Manager (nvm)

# Why all this hassle?

Arch Linux is a rolling release distribution, meaning that the latest and greatest software is pushed quickly (sometimes too quickly) into the 'pacman' package manager. The normal flow for a lot of arch users is to update their system daily and not really pay attention to the packages that are being updated. The NodeJS package is no exception. Whenever a new release of NodeJS is published, the version of nodejs used across all projects locally will be updated when updating the system and the user will probably not notice until it breaks one of their projects.

LTS versions of node can be installed, however, they are only currently two specific versions. Well what if a projects needs 'that specific version'? Well, your out of luck. Overall, managing node versions through the arch package manager is just not ideal.

## Why not just use nvm directly on the host machine?

Node Package Manager is a cli tool to change the local version of node being used. However, nvm is NOT part of the core arch linux repos. It can be found in the AUR (arch user repository), but AUR packages sometimes need tinkering to get running correctly and theres even less of a guarantee that the packages will remain stable. This approach is fine and I'm sure it works for many people, but also the small potential of manually removing any unwanted, abandoned, unstable, insecure, aur packages is more reason to avoid. Yes, arch linux is about tinkering, but trying to fix/debug nodejs problems can just be a straight up nightmare and a waist of time.

The alternative is to directly install the nvm to the system using their bash script. Firstly, bad practice from a security standpoint, but also that means nvm is no longer being managed by the arch package manager. This may seem okay at first but, what if the arch user installs a package where nodejs is a dependency? We are just gonna have two duplicates of nodejs laying around? I'm sure these are rarely problems, but the less dependency managers on the system, the less likely there are going to be conflicts and hours of fixing in the future.

## Benefits of the VM approach

1. nodejs and nvm is not directly installed on the host machine

If we do run into nodejs issues in the future, we can roll back to an earlier version of the virtual machine or just completely create a new virtual machine. Super fast, super simple. Sure, it is probably best practice to fix and understand nodejs instead of going full nuclear, but sometimes work just needs to get done. Going full nuclear may only take up to ~15 mins to fix problems while diagnosing problems can take a few hours to 'don't know how to fix it'.

2. keep proprietary software separate from the host machine

Let's be real, if you want to do web development, you pretty much NEED vscode. Or, spend hours configuring neovim and learning vim keybindings (vim is great for quick edits and other projects, just not web development).

Personally, I only allow core arch packages installed on my system. It keeps my system simple, easy to maintain, low security potentials, low potentials for malware and/or spyware, and I rarely have breaking issues. On top of that, I don't want microsoft's proprietary fingers on my host machine mucking around. Is this a little paranoid? Sure, but at the end of the day if I using proprietary software, I want a button that can make ALL of the software disappear without questions.

3. all your web projects are in one place

Do you have a million folders laying around with abandoned programming projects from various tech stacks, languages, and packages? Me too. Having all of your web dev projects in one VM is a great way to keep track and organize your projects. Also web projects tend to have a lot of dependencies and packages. Its kind of nice keeping these separate from the host machine.

4. transfer projects to different computer

If you do backups of your .qcow2 image, just transfer that file to a different computer and you have ALL of your web projects. BAM!

5. Tiling Window Manager

Using a tiling window manager for EVERYTHING can be very annoying. However, having it in a virtual machine just for coding web projects is a great excuse to have one. We will keep it very simple.

6. security

There have been instances where NPM packages contained malware. The likelihood of installing an NPM packages will malware is very small, but having that small potential happen inside a VM instead of the host machine is just another peace of mind. Also debian is just more secure than arch.

# Setup

## Install qemu and virt-manager

Follow this [Arch Wiki](https://wiki.archlinux.org/title/Virt-manager) guide

1. change the scaling

## Install debian to virtual machine

Go the the [Debian Downloads](https://www.debian.org/distrib/) page and click 'download mirrors' link.
Choose and download the mirror thats closest to your location.

Go forward with the default installation, but keep these notes in mind.

1. 4096 MB of RAM and 4 CPUs should be fine
2. we are going to call the root user debian (feel free to change to whatever you like)
3. create a user called webdev (feel free to change to whatever you like)
4. We are not going to install any desktop or system utils. We are going to install them manually later.

Using GNOME or KDE is fine, but I want to use a simple tiling window manager because it's less graphically intense, takes up less of a storage footprint, and the workflow is faster. Since we are just using it for simple code/commanding editing, we don't need to memorize and crazy keybindings, just five to get around (trust me its not bad and their pretty intuitive). Also we aren't going to edit any configs or dotfiles. Just the default i3.

Using sway would be nice, but we are going to stick to i3 because sway tends to have problems within virtual machines. Once sway starts working well inside VMs, transferring to sway should be super easy because i3 and sway are pretty much the same.

### Post Installation

Install some basic packages and add webdev to sudoers
Reboot virtual machine to apply changes

```
su debian
apt install sudo curl
sudo usermod -aG sudo webdev
sudo reboot
```

Create a Projects Directly in the home folder

```
mkdir ~/Projects
```

## Install and setup i3

```
sudo apt install i3
sudo reboot
```

After reboot, enter username and password.
Then enter the following command.

```
startx
```

A new i3 session should start
It's going to ask you basic config options. The defaults are fine.

### i3 basic commands

Remember, by default your super key is your windows key (unless you changed it)

1. super + enter -> new terminal
2. super + [1, 2, 3, 4, 5, etc] -> new window
3. super + d -> dmenu
4. super + shift + Q -> quit
5. super + shift + E -> exit i3

## Optional - Nicer Terminal Emulator

```bash
sudo apt install terminator
nano ~/.config/i3/config
```

change the following line to:

```bash
#start a terminal
bindsym $mod+Return exec terminator
```

## Optional - Install Homebrew

The stable Debian packages are... a little too stable. As in, Debian packages are a little dated. Take the hugo package as an example. As of writing this, the version of hugo in Debian is `0.111.0` while the current release version is `1.136.0`. Sure, there hasn't been a major version of hugo released, but there are many deprectation in themes we need to worry about. 

Brew is a package manager that is used on both Linux and MacOS. It's mostly geared toward MacOS, but it's mostly fine in Debian. The alternatives would be to build from source, use snaps or flatpaks, or use nix. All of those are fine solutions, but brew just gets things up and running the fastest. 

[Install Homebrew](https://brew.sh/)

## Optional - guest agents

If you want the virtual machine resolution to look a little nicer and adjust the host window. We can install the following guest agents

```
sudo apt install qemu-guest-agent
```

If the text becomes very small, its because your DPI is very large.

```
sudo nano /etc/X11/XResources/X11-common
```

Add the following line. Adjust the DPI to your needs

```
Xfc.dpi: 140
```

## Install vscode

[Official Documentation](https://wiki.debian.org/VisualStudioCode)

snaps is fine, but I prefer just using the proprietary key and install with apt because its easier to execute vscode in the terminal

```
code .
```

## Install nvm

just install via the curl command

[Official Documentation](https://github.com/nvm-sh/nvm)

## Serve the Website from the Guest Machine to Host Machine

The Virtual Machine was created with the NAT network interface. This means that the host has access to the guest ip address. We just need to make sure that we are serving our app on the guest network, not localhost.

First we need to figure out what our guest ip address is

```
ip addr
```

### NodeJS

```
npm run dev -- --host 192.168.x.x
```

### Hugo

```
hugo serve --bind 192.168.x.x --baseURL 192.168.x.x
```

# Conclusion

Welcome to the end. yes, this might of seemed like a lot of setup, but we only need to do it once! Hopefully this helped.
