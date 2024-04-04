#sudo docker build --network=host --tag=ubuntu:pi .
#sudo docker rm -f ubuntu; sudo docker run -d -u ubuntu  --net=host --dns=8.8.8.8 --restart always --mount type=bind,source=/data/data/com.termux/files/home/ubuntu,target=/home/ubuntu --mount type=bind,source=/data/export,target=/export --mount type=bind,source=/mnt/Airport_Data/,target=/mnt/Airport_Data --name ubuntu  --security-opt apparmor=unconfined --security-opt seccomp=unconfined --security-opt systempaths=unconfined ubuntu:pi
from ubuntu:latest

RUN \
    echo "nameserver 8.8.8.8" > /etc/resolv.conf &&\
    apt-get update && \
    apt-get install -y git-core gnupg flex bison build-essential zip curl zlib1g-dev x11proto-core-dev libx11-dev  libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig emacs-gtk sudo ncurses-dev libssl-dev bc python3 supervisor openssh-server remmina net-tools tmux repo

RUN \
    echo "nameserver 8.8.8.8" > /etc/resolv.conf &&\
    apt-get install -y tigervnc-xorg-extension xfonts-100dpi xfonts-75dpi xfonts-scalable xfwm4 tigervnc-standalone-server xfce4 xfce4-session xfce4-terminal software-properties-common dbus-x11 qbittorrent docker.io

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

RUN \
#    deluser ubuntu &&\
    groupadd -g 9997 everybody &&\
    groupadd -g 10147 u0_a147 &&\
    useradd -rm -d /home/ubuntu -s /bin/bash -g root -G sudo,9997,root,everybody,10147 -u 10147 -p "$(openssl passwd -1 kk)" ubuntu 

COPY ./configs/supervisord.conf /etc/

RUN \
    ssh-keygen -A &&\
    chmod +o+r -R /etc/supervisord.conf &&\
    chmod +o+r+x -R /etc/ssh/

ENV DISPLAY :2
# RUN \
#     /bin/bash -c "echo -e 'kk\nkk\nn' | vncpasswd"; echo; 

EXPOSE 9022/tcp


WORKDIR /home/ubuntu

CMD ["supervisord","-c", "/etc/supervisord.conf"]
#CMD exec /bin/bash -c "trap : TERM INT; sleep infinity & wait"
