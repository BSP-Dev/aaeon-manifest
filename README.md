This README file contains information on the contents of the meta-aaeon-nxp & meta-aaeon-mtk layer.
Please see the corresponding sections below for details.

# NXP SRG-IMX8P SRG-IMX8PL
### Package requirements
```bash!
sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib zstd liblz4-tool \ 
build-essential chrpath socat cpio python3 python3-pip python3-pexpect lz4 \ 
xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint xterm
```

### repo installation (This step may not be needed if it had already existed)
```bash!
$ mkdir ~/bin  
$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo  
$ chmod a+x \~/bin/repo  
$ export PATH=\~/bin:$PATH
```

## Download Yocto BSP with kernel 5.15.71 (kirkstone)
```bash!
   $ mkdir imx-yocto-bsp
   $ cd imx-yocto-bsp
   $ repo init -u git@github.com:BSP-Dev/aaeon-manifest.git -b kirkstone -m aaeon-kirkstone-v01.xml
   $ repo sync
```

## Build SRG-IMX8P BSP
   2G DDR <br />
```bash!
   $ DISTRO=fsl-imx-wayland MACHINE=srg-imx8p-2g source aaeon-imx-setup-release.sh -b imx8p_build
```
   4G DDR <br />
```bash!
   $ DISTRO=fsl-imx-wayland MACHINE=srg-imx8p-4g source aaeon-imx-setup-release.sh -b imx8p_build
   If you leave the build code environment, enter imx-yocto-bsp again by: <br />
   $ source setup-environment imx8p_build
```

### Build NXP IMX BSP
```bash!
   $ bitbake imx-image-full
```

## Build SRG-IMX8PL BSP
   SRG-IMX8PL 4GB <br />
```bash!
    $ DISTRO=fsl-imx-wayland MACHINE=srg-imx8pl-4g source aaeon-imx-setup-release.sh -b imx8p_build
```
   SRG-IMX8PL 2GB <br />
```bash!
    $ DISTRO=fsl-imx-wayland MACHINE=srg-imx8pl-2g source aaeon-imx-setup-release.sh -b imx8p_build
```

### Build NXP IMX BSP
```bash!
   $ bitbake imx-image-full
```
    
### Noted
- If you encounter a bitbake error from a recipe, try to re-build it. After building successfully, then build the imx-image-full again by:
```bash!
$ bitbake <package_name> -c do_cleansstate
$ bitbake -c compile <package_name>
$ bitbake imx-image-full
```
- If get FetchError message,then change git branch=master => branch=main.

### Flash Image into SDcard
- (1)	Go to Image Path by: <br>
```bash!
$ cd BUILD_DIR/tmp/deploy/images/<MACHINE_NAME>/
```
- (2)	Unzip .zst image file <br>
```bash!
$ unzstd imx-image-full-<MACHINE_NAME>-xxxxxx.rootfs.wic.zst
```
- (3)	Flash unzipped file named imx-image-full-<MACHINE_NAME>-xxxxxxx.rootfs.wic into SD-Card
- (4)	Use SD-Card Boot Mode to boot in SRG/PICO-IMX8P, SRG/PICO-IMX8PL

# MTK uCOM-M510
### Package requirements
- Setup Build Environment
```bash!
$ sudo apt-get install curl gawk wget git diffstat unzip texinfo gcc build-essential chrpath \
socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git \
python3-jinja2 libegl1-mesa libsdl1.2-dev pylint xterm python3-subunit mesa-common-dev liblz4-tool
```
- Setup Genio Tools
```bash!
$ sudo apt-get install android-tools-adb android-tools-fastboot
```
- USB Device rules
```bash!
$ echo -n 'SUBSYSTEM=="usb", ATTR{idVendor}=="0e8d", ATTR{idProduct}=="201c", MODE="0660", TAG+="uaccess"
SUBSYSTEM=="usb", ATTR{idVendor}=="0e8d", ATTR{idProduct}=="0003", MODE="0660", TAG+="uaccess"
SUBSYSTEM=="usb", ATTR{idVendor}=="0403", MODE="0660", TAG+="uaccess"
SUBSYSTEM=="gpio", MODE="0660", TAG+="uaccess"
' | sudo tee /etc/udev/rules.d/72-aiot.rules
```
```bash!
$ sudo udevadm control --reload-rules
$ sudo udevadm trigger
$ echo 'SUBSYSTEM=="usb", ATTR{idVendor}=="0e8d", ATTR{idProduct}=="201c", MODE="0660", $ GROUP="plugdev"' | sudo tee -a /etc/udev/rules.d/96-rity.rules
$ sudo udevadm control --reload-rules
$ sudo udevadm trigger
$ sudo usermod -a -G plugdev $USER
```
- Genio Tools
```bash!
$ pip3 install -U genio-tools
```
add your account to that group
```bash!
$ sudo usermod -a -G dialout $USER
```

### repo installation (This step may not be needed if it had already existed)
```bash!
$ mkdir ~/bin  
$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo  
$ chmod a+x \~/bin/repo  
$ export PATH=\~/bin:$PATH
```

## Download Yocto BSP with kernel 5.15 (kirkstone)
```bash!
   $ mkdir iot-yocto
   $ cd iot-yocto
   $repo init -u git@github.com:BSP-Dev/aaeon-manifest.git -b kirkstone -m aaeon-kirkstone-v02.xml --no-repo-verify
   $ repo sync
```

## Build uCOM-M510 BSP
   8G DDR <br />
```bash!
   $ source aaeon-mtk-setup-release.sh
   $ TEMPLATECONF=$PWD/src/meta-rity/meta/conf source src/poky/oe-init-build-env
   $ export BUILD_DIR=`pwd`
   $ MACHINE=genio-510-evk bitbake rity-demo-image -k
```

### Noted
- If you encounter a bitbake error from a recipe, try to re-build it. After building successfully, then build full image again by:
```bash!
$ bitbake <package_name> -c do_cleansstate
$ bitbake -c compile <package_name>
$ MACHINE=genio-510-evk bitbake rity-demo-image -k
```
- If get FetchError message,then change git branch=master => branch=main.

### Flash Image into eMMC
- Enter Download Mode
- Launch genio-flash Tool
```bash!
$ cd $BUILD_DIR/tmp/deploy/images/genio-510-evk
$ genio-flash
```
