# Installing CamFlow

There are three main options covered below for how to install CamFlow:
1. Fedora-specific: using packagecloud;
2. building the kernel on the local machine; or
3. using [vagrant](https://www.vagrantup.com/) to set up a virtual machine.

## 1. Installation via the package manager

The quickest way to install CamFlow is through the packages hosted on [packagecloud](https://packagecloud.io/camflow/provenance). For now, only Fedora and Ubuntu are supported (please, click on this [link](https://packagecloud.io/camflow/provenance) and check which release(s) is/are currently supported). We describe next how to install CamFlow and its dependencies on Fedora and Ubuntu.

### Fedora

``` BASH
curl -s https://packagecloud.io/install/repositories/camflow/provenance/script.rpm.sh | sudo bash
sudo dnf -y install camflow
```

### Ubuntu
```BASH
curl -s https://packagecloud.io/install/repositories/camflow/provenance/script.deb.sh | sudo bash
sudo apt-get install -y libprovenance=0.4.5-2
sudo apt-get install -y camflowd=0.2.3-2
sudo apt-get install -y camflow-cli=0.1.12-2
sudo apt-get install -y camconfd=0.4.4-2
sudo apt-get install -y linux-libc-dev=4.20.7camflow0.5.1+-1
sudo apt-get install -y linux-image-4.20.7camflow0.5.0+=4.20.5camflow0.5.1+-1
sudo apt-get install -y linux-headers-4.20.7camflow0.5.0+=4.20.5camflow0.5.1+-1
```

**Note:** check [online](https://packagecloud.io/camflow/provenance) to find the latest version number and update the above script accordingly.

### General


Next we need to activate the two CamFlow services:

``` BASH
sudo systemctl enable camconfd.service
sudo systemctl enable camflowd.service
```

After reboot we should be ready to use CamFlow.

``` BASH
sudo reboot now
```

**Note:** We are a small research team. Packages are tested in virtual environment running over a limited set of hardware configurations. In most cases things work fine. However, if you encounter any issue, please do look at how to build the project from source.

## 2. Building from source

### Dependencies

First we need to install the dependencies required to build our kernel.

Depending on how recent your OS version is, you should install `libelf-dev`, `libelf-devel`, or `elfutils-libelf-devel`.
See this [issue](https://github.com/CamFlow/documentation/issues/3) for details.

#### Fedora

``` BASH
sudo dnf groupinstall 'Development Tools'
sudo dnf install ncurses-devel cmake clang gcc-c++ wget git openssl-devel zlib patch mosquitto bison flex ruby
```

For Fedora 28 and above, also run:

``` BASH
sudo dnf install elfutils-libelf-devel
```

#### Ubuntu

``` BASH
sudo apt-get -y install build-essential
sudo apt-get -y install libncurses-dev cmake clang g++ wget git libssl-dev bc nano patch mosquitto bison flex ruby
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

**Note:** configuration options need to be carefully considered in resource constrained enviornment.

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


**Note:** running virtual machines is resource consuming. Please, make sure the host has sufficient resource to do so (disk space, RAM, CPU etc.).

## After Reboot

When booting a VM after successful provisioning, ensure that the CamFlow kernel is chosen in the [GRUB](https://www.gnu.org/software/grub/) menu.
