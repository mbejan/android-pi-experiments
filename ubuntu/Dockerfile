from ubuntu:22.04

RUN \
    echo "nameserver 8.8.8.8" > /etc/resolv.conf &&\
    apt-get update && \
    apt-get install -y git-core gnupg flex bison build-essential zip curl zlib1g-dev libncurses5 x11proto-core-dev libx11-dev  libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig emacs-nox sudo ncurses-dev libssl-dev bc python3

RUN mkdir -p /work/home/
RUN useradd -rm -d /work/home/ubuntu -s /bin/bash -g root -G sudo -u 1001 -p "$(openssl passwd -1 kk)" ubuntu
RUN \
    echo "nameserver 8.8.8.8" > /etc/resolv.conf &&\
    apt-get install -y tigervnc-xorg-extension xfonts-100dpi xfonts-75dpi xfonts-scalable xfwm4 tigervnc-standalone-server xfce4 xfce4-session xfce4-terminal software-properties-common

RUN \
    echo "nameserver 8.8.8.8" > /etc/resolv.conf &&\
    sudo add-apt-repository -y ppa:mozillateam/ppa

RUN \
    echo "nameserver 8.8.8.8" > /etc/resolv.conf &&\
    sh -c """echo ' \
Package: * \n\
Pin: release o=LP-PPA-mozillateam \n\
Pin-Priority: 1001 \n\
Package: firefox \n\
Pin: version 1:1snap1-0ubuntu2 \n\ 
Pin-Priority: -1 \n\
    ' | sudo tee /etc/apt/preferences.d/mozilla-firefox""" &&\
    apt-get install -y firefox 


COPY ./bins/icaclient_23.2.0.10_armhf.deb /tmp/

ENV DEBIAN_FRONTEND=noninteractive

RUN \
    echo "nameserver 8.8.8.8" > /etc/resolv.conf &&\
    dpkg --add-architecture armhf &&\
    apt update -y &&\
    apt install -y /tmp/icaclient_23.2.0.10_armhf.deb &&\
    rm -rf /tmp/*

RUN \
    echo "nameserver 8.8.8.8" > /etc/resolv.conf &&\
    apt-get install -y wget gpg &&\
    wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg &&\
    install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg &&\
    sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list' &&\
    rm -f packages.microsoft.gpg &&\
    apt -y install apt-transport-https &&\
    apt -y update &&\
    apt -y install code

ENV DISPLAY :2
#/bin/bash -c "echo -e 'kk\nkk\nn' | vncpasswd"; echo; \

#USER ubuntu
WORKDIR /work/home/ubuntu

#ENTRYPOINT ["vncserver","-xstartup", "/usr/bin/xfce4-session", "-verbose", "-localhost","no","-geometry","1280x720", "-fg", ":2"]
ENTRYPOINT ["vncserver","-xstartup", "/usr/bin/xfce4-session", "-verbose", "-localhost","no","-geometry","1920x1080", "-fg", ":2"]
