# Installing CamFlow

There are three main options covered below for how to install CamFlow:
1. Fedora-specific: using packagecloud;
2. building the kernel on the local machine; or
3. using [vagrant](https://www.vagrantup.com/) to set up a virtual machine.

## 1. Installation via the package manager

The quickest way to install CamFlow is through the packages hosted on [packagecloud](https://packagecloud.io/camflow/provenance). For now, only Fedora is supported. The installation process is really simple

``` BASH
curl -s https://packagecloud.io/install/repositories/camflow/provenance/script.rpm.sh | sudo bash
sudo dnf install camflow
```

Next we need to activate the two CamFlow services:

``` BASH
sudo systemctl enable camconfd.service
sudo systemctl enable camflowd.service
```

After reboot we should be ready to use CamFlow.

``` BASH
sudo reboot now
```

## 2. Building from source

### Dependencies

First we need to install the dependencies required to build our kernel.

#### Fedora

``` BASH
sudo dnf groupinstall 'Development Tools'
sudo dnf install ncurses-devel cmake clang gcc-c++ wget git openssl-devel zlib patch mosquitto
```

#### Ubuntu

``` BASH
sudo apt-get -y install build-essential
sudo apt-get -y install libncurses-dev cmake clang g++ wget git libssl-dev bc nano patch mosquitto
```

### Building and Installing the kernel

We first need to clone the `camflow-install` repository:

``` BASH
git clone https://github.com/CamFlow/camflow-install
```

We then get the installation started:
``` BASH
cd camflow-install
make all
```

This will build and install the CamFlow Linux Security Module as well as the userspace tools. The whole installation procedure may take a significant amount of time. The installation process may ask for the root password, so may not complete in an unattended manner.

The kernel configuration derives from the configuration currently present on the system where you run the build. Early in the build process you will be presented with a GUI to customise the kernel configuration. If you are not sure what to do, do not modify the configuration.

For the installation process to take effect you need to reboot the machine.

``` BASH
sudo reboot now
```

## 3. Setting up a vagrant virtual machine

Using a vagrant virtual machine is much simpler. First you need to install [vagrant](https://www.vagrantup.com/docs/installation/) and [virtualbox](https://www.virtualbox.org/manual/ch02.html).

On Ubuntu, for example, that can be done as follows:
``` BASH
sudo apt-get install virtualbox
sudo apt-get install vagrant
```

**Note:** Some Linux distributions ship very outdated version of VirtualBox. It is recommended to use the most recent version. This can be downloaded from [their website](https://www.virtualbox.org/wiki/Downloads). Outdated versions, and host/guest version mismatch are known to cause all sorts of troubles during provisioning.

After the installations you will see some directories, for example:
``` BASH
vagrant, vagrant-master, virtualbox-5.1_5.1.28-117968~Ubuntu~xenial_amd64.deb
```
The vagrant directory contains:
``` BASH
LICENSE, README.md, basic-fedora, dev-fedora, docker-fedora, light, rpm
```
You will be working from the rpm directory, as shown in the code below.

Once vagrant and virtualbox are installed, you have to install a Linux kernel in the virtualbox and then install the CamFlow LSM within it. This process is called "provisioning". After that, you must halt the execution in the virtualbox and select the new kernel that contains the CamFlow LSM:

``` BASH
git clone https://github.com/CamFlow/vagrant.git
cd ./vagrant/rpm
vagrant plugin install vagrant-vbguest
vagrant up
# we reboot after the provisioning
vagrant halt
vagrant up
```

When booting a VM after successful provisioning, ensure that the CamFlow kernel is chosen in the [GRUB](https://www.gnu.org/software/grub/) menu:
![Select CamFlow kernel in the grub menu](http://camflow.org/img/grub.png)
