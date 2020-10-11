---
title: New Ubuntu install
categories: linux
tags:
    - ubuntu
    - linux
    - install
    - apt

date: 2019-07-22
---

Yeah, I tried. Again. For reasons that are not important, I had to install Windows again on my personal laptop. And this time I said, yeah, let's try it and see if I can use it as my main machine. Note, I'm not working heavily from this machine, mostly personal projects, media and hobbies. But anyway, it lasted 10 days. First, Python is still a hassle on Windows. And no conda here, thank you very much. Second, and probably mainly, I was already invested in Linux before, as my secondary disk was formatted as EXT4, with all my photos, for example. It's 2019 and there is no way to safely read and write EXT4 from Windows. Finally, it feels... *awkward*. I don't know how to express myself, but everything that I had to do was... convoluted. 

So, back to Ubuntu. I thought about using Debian Buster but the already old versions of Gnome and KDE were too old for what I want in my personal desktop. So Ubuntu 19.04 it is. So what I had to do to get my new install into a usable way? But before that, a small digression: I'm a long time KDE user, but after a discussion with some friends, decided to give the Ubuntu's Gnome flavor a spin, including Wayland. So far so good.

## Packages

I compiled a list of packages that I installed:

- vim
- digikam (removed, more on that later)
- texstudio
- vlc
- intel firmware (installed at OS install time)
- thunderbird
- backintime
- keepassxc
- gnucash
- transmission  
- qt5ct
- mesa-utils
- htop
- darktable
- clock-override (gnome extension)
- epson-inkjet-printer-escpr
- libreoffice-writer
- libreoffice-calc

And so far, these are the only packages that I installed using `apt`. They cover a good part of my workflow. But not all, unfortunately. I had to install some packages from outside. Digikam was one example: upload and download images from Google Photos after the Picasa API was deprecated is only possible with Digikam version 6 and higher, and the latest version packaged on Ubuntu 19.04 is 5.9.0. So I went and downloaded the AppImage package, that seems to be the "new" way that the KDE folks are supporting to install applications. 

But I still had other packages missing, and I had to install downloading them directly from their websites.

## External packages

I had to install some packages from outside Ubuntu repos. They are:

- cryptomator (PPA)
- spotify
- google chrome
- insync
- vscode

As I will not use Snaps, most of these packages were either .deb downloaded directly or added an external repository. Not the best way, but so far it's the only way.

## Configuration

Then it came the configuration changes to make the system usable for me. In general there were not many. 

### Graphical changes

Two main changes: one to make the Dash to Dock work with minimize with click,

```
gsettings set org.gnome.shell.extensions.dash-to-dock click-action 'minimize'
```

and make the Qt applications on Gnome show icons:

```
echo "export QT_QPA_PLATFORMTHEME=qt5ct" >> ~/.profile 
```

### Networking

I had a very [fun](https://askubuntu.com/questions/1159084/make-dns-follow-configuration-from-network-manager-in-19-04/) discussion on Stack Overflow about how to configure DNS servers with Network Manager and not use `systemd-resolve`. Finally managed to fix it by myself - see the answer there.

I also disabled ipv6. Edit `/etc/default/grub` and change the following line:

```
GRUB_CMDLINE_LINUX="ipv6.disable=1"
```

## Swap

Ubuntu does not ask to create a swap partition anymore if the custom partitioner is used. It creates by default a swap file and I don't want to use swap, so I just disabled the swap line that mounts `/swapfile` on the `/etc/fstab` file. 

## Local directories

I actually use a lot the XDG definitions for files, but I have my own layout for these files. The solution is to edit the `$HOME/.config/config-dirs.dirs` file and set what is desired. As a bit of warning, if there's a path that does not exist in this file, Nautilus will crash without warning or error message!

## Conclusion

I've still getting all pieces together, but so far things are working. I'm collecting also the configuration files for the applications themselves, like VSCode, and thinking about how to automate this setup.
