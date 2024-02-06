#sudo docker build --network=host --tag=ubuntu:pi .
#sudo docker rm -f ubuntu; sudo docker run -d -u ubuntu  --net=host --dns=8.8.8.8 --restart always --mount source=ubuntu,target=/work --mount type=bind,source=/data/export,target=/export --mount type=bind,source=/mnt/Airport_Data/,target=/mnt/Airport_Data --name ubuntu  --security-opt apparmor=unconfined --security-opt seccomp=unconfined --security-opt systempaths=unconfined ubuntu:pi
from ubuntu:23.10

RUN \
    echo "nameserver 8.8.8.8" > /etc/resolv.conf &&\
    apt-get update && \
    apt-get install -y git-core gnupg flex bison build-essential zip curl zlib1g-dev x11proto-core-dev libx11-dev  libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig emacs sudo ncurses-dev libssl-dev bc python3 supervisor openssh-server

RUN mkdir -p /work/home/
RUN \
    deluser ubuntu &&\
    useradd -rm -d /work/home/ubuntu -s /bin/bash -g root -G sudo -u 1001 -p "$(openssl passwd -1 kk)" ubuntu
RUN \
    echo "nameserver 8.8.8.8" > /etc/resolv.conf &&\
    apt-get install -y tigervnc-xorg-extension xfonts-100dpi xfonts-75dpi xfonts-scalable xfwm4 tigervnc-standalone-server xfce4 xfce4-session xfce4-terminal software-properties-common dbus-x11

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
Pin: release o=Ubuntu \n\ 
Pin-Priority: -1 \n\
    ' | sudo tee /etc/apt/preferences.d/mozilla-firefox""" &&\
    apt-get install -t 'o=LP-PPA-mozillateam' -y firefox 


ENV DEBIAN_FRONTEND=noninteractive

# RUN \
#     echo "nameserver 8.8.8.8" > /etc/resolv.conf &&\
#     dpkg --add-architecture armhf &&\
#     apt update -y &&\
#     rm -rf /tmp/*

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

ENTRYPOINT ["vncserver","-xstartup", "/usr/bin/xfce4-session", "-verbose", "-localhost","no","-geometry","1920x1080", "-fg", ":2"]
