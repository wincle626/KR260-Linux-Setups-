# The is the repository for setting up KR260 linux with ROS

<img title="a title" alt="Alt text" src="https://github.com/wincle626/KR260-Linux-Setups-/blob/main/images/20240623_200214.jpg">

## Prepare SD card

### 1. Use SD card image from Ubuntu

#### a. Download [balena etcher application](https://etcher.balena.io/)

<img title="a title" alt="Alt text" src="https://github.com/wincle626/KR260-Linux-Setups-/blob/main/images/Screenshot%20from%202024-06-23%2021-03-26.png">

Using commande chmod +x balenaEtcher-1.18.11-x64.AppImage to enable the executable of balena etcher application. 

#### b. Download the [ubuntu image](https://ubuntu.com/download/amd) and copy to SD card

<img title="a title" alt="Alt text" src="https://github.com/wincle626/KR260-Linux-Setups-/blob/main/images/Screenshot%20from%202024-06-23%2021-05-04.png">

Using command xz -d iot-limerick-kria-classic-desktop-2204-20240304-165.img.xz to unzip the compressed ubuntu image. 

Run the belane etcher application and choose the image and the target SD card drive, then click on the Flash! to copy all image content to the SD card.

<img title="a title" alt="Alt text" src="https://github.com/wincle626/KR260-Linux-Setups-/blob/main/images/Screenshot%20from%202024-06-23%2022-58-13.png">

#### c. Chage the password policy to enable simple password

Open the prepared SD card folder in file blowser and locate the file /etc/pam.d/common-password. Use the command sudo gedit /etc/pam.d/common-password to open it in root privilage and remove the "obscure" options of password. So when boot from SD card, it will not ask for changing password to a complexed one. By default, the account of Ubuntu image is: ubuntu, while the password is the same: ubuntu. 

#### (PS: this method works for both KR260 and KV260. Ubuntu provides the universial Ubuntu image for these two devices. )

### 2. Rebuilding the Certified Ubuntu for Xilinx Devices Kernel from Source (On KR260)

#### a. Clone Ubuntu source to local

git clone https://git.launchpad.net/~canonical-kernel/ubuntu/+source/linux-xilinx-zynqmp/+git/jammy 

(PS:jammy refers to Ubuntu 22.04, replace jammy with focal for Ubuntu 20.04)

#### b. Checkout specific tag

cd jammy

git checkout Ubuntu-xilinx-zynqmp-5.15.0-1032.36

#### c. Install prerequisite

sudo apt install libncurses-dev gawk flex bison openssl libssl-dev dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf python-docutils asciidoc default-jdk dwarves

#### d. Change kernel settings

export ARCH=arm64

export $(dpkg-architecture -aarm64)

(PS: if using cross compile on the host PC, add "export CROSS_COMPILE=aarch64-linux-gnu-" before building the kernel)

fakeroot debian/rules clean

fakeroot debian/rules editconfigs

<img title="a title" alt="Alt text" src="https://github.com/wincle626/KR260-Linux-Setups-/blob/main/images/Screenshot%202024-06-25%20093600.png">

#### e. Build kernel packages

fakeroot debian/rules clean

fakeroot debian/rules binary

#### f. Install new kernel packages
