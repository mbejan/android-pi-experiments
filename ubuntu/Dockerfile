#sudo docker build --network=host --tag=ubuntu:pi .
#sudo docker rm -f ubuntu; sudo docker run -d -u ubuntu  --net=host --dns=8.8.8.8 --restart always --mount type=bind,source=/data/data/com.termux/files/home/ubuntu,target=/home/ubuntu --mount type=bind,source=/data/export,target=/export --mount type=bind,source=/mnt/Airport_Data/,target=/mnt/Airport_Data --name ubuntu  --security-opt apparmor=unconfined --security-opt seccomp=unconfined --security-opt systempaths=unconfined ubuntu:pi
from ubuntu:22.04
#from ubuntu:24.04


RUN \
    echo "nameserver 8.8.8.8" > /etc/resolv.conf &&\
    apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y git-core gnupg flex bison build-essential zip curl zlib1g-dev x11proto-core-dev libx11-dev  libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig emacs-gtk sudo ncurses-dev libssl-dev bc python3 supervisor openssh-server remmina net-tools tmux repo xfce4-goodies gvfs-backends smbclient ubuntu-restricted-extras pulseaudio ffmpeg openjdk-11-jdk doublecmd-gtk calibre


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

##xrdp
RUN \
    echo "nameserver 8.8.8.8" > /etc/resolv.conf &&\
    apt-get install -y git autoconf libtool pkg-config gcc g++ make  libssl-dev libpam0g-dev \
             libjpeg-dev libx11-dev libxfixes-dev libxrandr-dev  flex bison libxml2-dev \
             intltool xsltproc xutils-dev python3-libxml2 g++ xutils libfuse-dev \
             libmp3lame-dev nasm libpixman-1-dev xserver-xorg-dev libfdk-aac-dev libopus-dev libpulse-dev
RUN \
    cd /tmp &&\
    echo "nameserver 8.8.8.8" > /etc/resolv.conf &&\
    git clone https://github.com/neutrinolabs/xrdp.git &&\
    cd xrdp &&\
    ./bootstrap &&\
    ./configure --with-sound --enable-pixman --with-freetype2 --with-fdkaac --enable-fdkaac --enable-vsock --enable-mp3lame --enable-opus --enable-fuse --enable-txfcodec --enable-painter --enable-rdpsndaudin &&\
    make install &&\
    rm -rf /tmp/xrdp

#xorgxrdp
RUN \
    cd /tmp &&\
    echo "nameserver 8.8.8.8" > /etc/resolv.conf &&\
    git clone https://github.com/neutrinolabs/xorgxrdp.git &&\
    cd xorgxrdp &&\
    ./bootstrap &&\
    ./configure &&\
    make install &&\
    sed -i 's/console/anybody/g' /etc/X11/Xwrapper.config  &&\
    rm -rf /tmp/xorgxrdp

#audio
COPY packages/pulseaudio-module-xrdp /tmp/pulseaudio-module-xrdp

RUN \
    cd /tmp/pulseaudio-module-xrdp/src/ &&\
    /usr/bin/mkdir -p '/usr/lib/pulse-15.99.1+dfsg1/modules' &&\
    /bin/bash ../libtool   --mode=install /usr/bin/install -c   module-xrdp-sink.la module-xrdp-source.la '/usr/lib/pulse-15.99.1+dfsg1/modules' &&\
    /usr/bin/install -c .libs/module-xrdp-sink.so /usr/lib/pulse-15.99.1+dfsg1/modules/module-xrdp-sink.so &&\
    /usr/bin/install -c .libs/module-xrdp-sink.lai /usr/lib/pulse-15.99.1+dfsg1/modules/module-xrdp-sink.la &&\
    /usr/bin/install -c .libs/module-xrdp-source.so /usr/lib/pulse-15.99.1+dfsg1/modules/module-xrdp-source.so &&\
    /usr/bin/install -c .libs/module-xrdp-source.lai /usr/lib/pulse-15.99.1+dfsg1/modules/module-xrdp-source.la &&\
    cd /tmp/pulseaudio-module-xrdp/instfiles &&\
    /usr/bin/mkdir -p '/usr/libexec/pulseaudio-module-xrdp' &&\
    /usr/bin/install -c load_pa_modules.sh '/usr/libexec/pulseaudio-module-xrdp' &&\
    /usr/bin/mkdir -p '/etc/xdg/autostart' &&\
    /usr/bin/install -c -m 644 pulseaudio-xrdp.desktop '/etc/xdg/autostart'


RUN \
#    deluser ubuntu &&\
    groupadd -g 9997 everybody &&\
    groupadd -g 10147 u0_a147 &&\
    useradd -rm -d /home/ubuntu -s /bin/bash -g root -G sudo,9997,root,everybody,10147 -u 10147 -p "$(openssl passwd -1 kk)" ubuntu &&\
    echo "ubuntu ALL=(ALL:ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/ubuntu

COPY ./configs/supervisord.conf /etc/
COPY ./configs/xrdp.ini /etc/xrdp/

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
