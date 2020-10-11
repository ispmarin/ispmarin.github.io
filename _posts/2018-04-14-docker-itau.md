---
title: Creating a dockerfile to access an internet banking
categories: docker
tags:
    - OS
    - linux
    - containers
date: 2018-04-14
---

It's a bit absurd what I had to do to get an online internet banking working. The bank has a "Warsaw Guardian"
thing that needs to be installed as root, keeps running in the background and monitors everything that you do
in your PC. No thanks. 

Docker came to the rescue! I was able to create a dockerfile, install the dreaded software and run firefox from it,
so now I can access the home banking site without having to worry that this program will spy on everything in my machine.
The dockerfile is below:

```
FROM debian:stretch

RUN apt-get update && apt-get install sudo

RUN export uid=1000 gid=1000 && \
    mkdir -p /home/ispmarin && \
    echo "ispmarin:x:${uid}:${gid}:Developer,,,:/home/ispmarin:/bin/bash" >> /etc/passwd && \
    echo "ispmarin:x:${uid}:" >> /etc/group && \
    echo "ispmarin ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/ispmarin && \
    chmod 0440 /etc/sudoers.d/ispmarin && \
    chown ${uid}:${gid} -R /home/ispmarin

RUN apt-get install -y xterm firefox-esr curl libnss3-tools
RUN curl -o /tmp/warsaw_setup_64.deb 'https://guardiao.itau.com.br/warsaw/warsaw_setup_64.deb'
RUN dpkg -i /tmp/warsaw_setup_64.deb

ENV DISPLAY :0
RUN echo 'deb http://ftp.br.debian.org/debian/ stretch main contrib non-free'  >> /etc/apt/sources.list
RUN apt-get update
RUN echo 'ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula select true' | sudo debconf-set-selections
RUN apt-get install -y libfreetype6 libfreetype6-dev libfontconfig ttf-mscorefonts-installer

USER ispmarin
ENV HOME /home/ispmarin

CMD sudo /etc/init.d/warsaw start && /usr/bin/firefox -no-remote
```

Quite simple. To run it, remember to map a local folder to be able to get any exported file:

```
docker build -t itau .
docker run --rm -v /tmp/.X11-unix:/tmp.X11-unix -v /home/ispmarin/Downloads:/home/ispmarin/downloads itau
```

and done!

