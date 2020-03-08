# Credits to Jessie Frazelle !
FROM ubuntu:bionic
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install -y \
	libcurl4 \
	libvpx5 \
	procps \
	--no-install-recommends \
	&& rm -rf /var/lib/apt/lists/*

RUN buildDeps=' \
		ca-certificates \
		curl \
		gnupg \
	' \
	&& set -x \
	&& mkdir -p /etc/xdg/QtProject \
	&& apt-get update && apt-get install -y $buildDeps --no-install-recommends \
	&& rm -rf /var/lib/apt/lists/* \
	&& curl -sSL https://www.virtualbox.org/download/oracle_vbox_2016.asc | apt-key add - \
	&& echo "deb http://download.virtualbox.org/virtualbox/debian bionic contrib" >> /etc/apt/sources.list.d/virtualbox.list \
	&& apt-get update && apt-get install -y \
	virtualbox-6.1 \
	--no-install-recommends \
    && VBoxVer=`dpkg -s virtualbox-6.1 | grep "^Version" | cut -d " " -f 2 | cut -d "-" -f 1` \
    && curl --insecure https://download.virtualbox.org/virtualbox/$VBoxVer/Oracle_VM_VirtualBox_Extension_Pack-$VBoxVer.vbox-extpack --output /tmp/Oracle_VM_VirtualBox_Extension_Pack-$VBoxVer.vbox-extpack \
	&& mkdir -p /usr/lib/virtualbox/ExtensionPacks/Oracle_VM_VirtualBox_Extension_Pack/ \
	&& tar xzf /tmp/Oracle_VM_VirtualBox_Extension_Pack-$VBoxVer.vbox-extpack -C /usr/lib/virtualbox/ExtensionPacks/Oracle_VM_VirtualBox_Extension_Pack/ \
	&& chmod -R o-w /usr/lib/virtualbox/ExtensionPacks/Oracle_VM_VirtualBox_Extension_Pack \
	&& apt-get purge -y --auto-remove $buildDeps

# Create a VM
RUN VBoxManage createvm --name WinNT --ostype WindowsNT4 --register
RUN VBoxManage modifyvm WinNT --memory 128
RUN VBoxManage storagectl WinNT --name "IDE Controller" --add ide
RUN VBoxManage modifyvm WinNT --boot1 disk --boot2 dvd --boot3 none --boot4 none
RUN VBoxManage modifyvm WinNT --vrde on --vrdeport 3389
RUN VBoxManage modifyvm WinNT --vram 32

# solves vbox scripts from sending all kinds of warnings
RUN apt-get install -y kmod
