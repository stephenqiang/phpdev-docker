FROM debian

RUN sed -i "s@http://deb.debian.org@http://mirrors.aliyun.com@g" /etc/apt/sources.list && rm -Rf /var/lib/apt/lists/*

RUN apt-get update --fix-missing
#RUN apt-get install -y wget && wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && bash install.sh

RUN mkdir /www

WORKDIR /www