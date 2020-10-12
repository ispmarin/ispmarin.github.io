---
title: Disabling services in systemd weird user folder
categories: Linux
date: 2020-05-10
tags:
  - linux
  - manjaro
  - systemd
---

I'm currently trying [Manjaro](https://manjaro.org) after a long time with Debian, after being convinced by a friend. So far, so good! The rolling release aspect of it is great, while it seems to be stable. But the system packages have some deep dependencies, like Network Manager depending on a lot of GVFS services. I really don't like services running that I don't need, and because of the package dependency, I was not able to remove them. So I decided to disable the services instead of removing the packages. And here Systemd came again to give me headaches. 

The obvious place to see where the services were being enabled is `/etc/init.d`, but this folder does not exist. Ok, this is really a Systemd distro. Ok, next place is `/lib/systemd/` (what a weird place) and there are not services for GVFS here. I had to dig more to find that packages install their init files on `/lib/systemd/user`. A folder called `user` for system packages. 

Anyway, I tried to disable these services with `--user`, the obvious choice, but no luck. Same thing with `--global`. So I decided to nuke them from orbit and just delete the files from this folder. Finally, it works. For example, to remove GVFS-gphoto, I removed the .service file:

```
sudo rm /lib/systemd/user/gvfs-gphoto2-whatever.service
```

and no more gphoto background running.