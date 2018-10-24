# Installing CamFlow

There are three main options covered below for how to install CamFlow:
1. Fedora-specific: using packagecloud;
2. building the kernel on the local machine; or
3. using [vagrant](https://www.vagrantup.com/) to set up a virtual machine.

## 1. Installation via the package manager

The quickest way to install CamFlow is through the packages hosted on [packagecloud](https://packagecloud.io/camflow/provenance). For now, only Fedora is supported (please, click on this [link](https://packagecloud.io/camflow/provenance) and check which Fedora release(s) is/are currently supported). The installation process is really simple

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

Depending on how recent your OS version is, you should install `libelf-dev`, `libelf-devel`, or `elfutils-libelf-devel`.
See this [issue](https://github.com/CamFlow/documentation/issues/3) for details.

#### Fedora

``` BASH
sudo dnf groupinstall 'Development Tools'
sudo dnf install ncurses-devel cmake clang gcc-c++ wget git openssl-devel zlib patch mosquitto bison flex
```

For Fedora 28 and above, also run:

``` BASH
sudo dnf install elfutils-libelf-devel
```

#### Ubuntu

``` BASH
sudo apt-get -y install build-essential
sudo apt-get -y install libncurses-dev cmake clang g++ wget git libssl-dev bc nano patch mosquitto bison flex
```

On more recent Ubuntu releases you should also run:

``` BASH
sudo apt-get -y install libelf-dev
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

You can install older release of CamFlow as follow:

``` BASH

git clone https://github.com/CamFlow/camflow-install
cd camflow-install
make v0.4.2
sudo reboot now
```

Repalce `v0.4.2` by your desired target version.
Please, note that only the most recent release is actively supported.

## 3. Setting up a vagrant virtual machine

Using a vagrant virtual machine is much simpler. First you need to install [vagrant](https://www.vagrantup.com/docs/installation/) and [virtualbox](https://www.virtualbox.org/manual/ch02.html).

On Ubuntu, for example, that can be done as follows:
``` BASH
sudo apt-get install virtualbox
sudo apt-get install vagrant
```

**Note:** Some Linux distributions ship very outdated version of VirtualBox. It is recommended to use the most recent version. This can be downloaded from [their website](https://www.virtualbox.org/wiki/Downloads). Outdated versions, and host/guest version mismatch are known to cause all sorts of troubles during provisioning.

Please check [Vagrant](https://www.vagrantup.com/downloads.html) and [VirtualBox](https://www.virtualbox.org/wiki/Downloads) for more installation details.

Once vagrant and virtualbox are installed, you need to obtain CamFlow vagrant provision script:
``` BASH
git clone https://github.com/CamFlow/vagrant.git
cd ./vagrant
```

There are different provisioning scripts available within the `CamFlow/vagrant.git` repository: please see in the [camflow/vagrant](https://github.com/CamFlow/vagrant) for details on what they do. We will use the `rpm` provisioning script as an example:
``` BASH
cd ./rpm
vagrant plugin install vagrant-vbguest
vagrant up
# we reboot after the provisioning
vagrant halt
vagrant up
```

## After Reboot

When booting a VM after successful provisioning, ensure that the CamFlow kernel is chosen in the [GRUB](https://www.gnu.org/software/grub/) menu:
![Select CamFlow kernel in the grub menu](http://camflow.org/img/grub.png)
