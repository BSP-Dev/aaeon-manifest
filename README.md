This README file contains information on the contents of the meta-aaeon-imx8p layer.

Please see the corresponding sections below for details.
# Package requirements
sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib zstd liblz4-tool \ 
build-essential chrpath socat cpio python3 python3-pip python3-pexpect lz4 \ 
xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint xterm

# repo installation (This step may not be needed if it had already existed)
$ mkdir ~/bin  
$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo  
$ chmod a+x ~/bin/repo  
$ export PATH=~/bin:$PATH  

# Build SRG-IMX8P BSP
## (1)	Download Yocto BSP with kernel 5.15.71
   $ mkdir imx-yocto-bsp <br />
   $ cd imx-yocto-bsp <br />
   $ repo init -u https://github.com/jasonfsyang/aaeon-nxpimx8-manifest -b main -m aaeon-kirkstone-v01.xml <br />
   $ repo sync
## (2)	Environment setup
   2G DDR <br />
   $ DISTRO=fsl-imx-wayland MACHINE=srg-imx8p-2g source aaeon-imx-setup-release.sh -b imx8p_build <br />
   4G DDR <br />
   $ DISTRO=fsl-imx-wayland MACHINE=srg-imx8p-4g source aaeon-imx-setup-release.sh -b imx8p_build <br />
   If you leave the build code environment, enter imx-yocto-bsp again by: <br />
   $ source setup-environment imx8p_build

## (3)	Build NXP IMX BSP
   $ bitbake imx-image-full


# Build SRG-IMX8PL BSP
## (1)   Download Yocto BSP with kernel 5.15.71
   $ mkdir imx-yocto-bsp <br />
   $ cd imx-yocto-bsp <br />
   $ repo init -u https://github.com/jasonfsyang/aaeon-manifest.git -b main -m aaeon-kirkstone-v02.xml <br />
   $ repo sync
## (2)   Environment setup
   SRG-IMX8PL 4GB <br />
    $ DISTRO=fsl-imx-wayland MACHINE=srg-imx8pl-4g source aaeon-imx-setup-release.sh -b imx8p_build <br />
   SRG-IMX8PL 2GB <br />
    $ DISTRO=fsl-imx-wayland MACHINE=srg-imx8pl-2g source aaeon-imx-setup-release.sh -b imx8p_build

## (3)   Build NXP IMX BSP
   $ bitbake imx-image-full
## (4)   Flash to SD Card
    
### Noted
### If you encounter a bitbake error from a recipe, try to re-build it. After building successfully, then build the imx-image-full again by:
$ bitbake <package_name> -c do_cleansstate <br />
$ bitbake -c compile <package_name> <br />
$ bitbake imx-image-full <br />
### If get FetchError message,then change git branch=master => branch=main.

# Flash Image into SDcard
## (1)	Go to Image Path by: <br>
$ cd BUILD_DIR/tmp/deploy/images/<MACHINE_NAME>/ <br>
## (2)	Unzip .zst image file <br>
$ unzstd imx-image-full-<MACHINE_NAME>-xxxxxx.rootfs.wic.zst <br>
## (3)	Flash unzipped file named imx-image-full-<MACHINE_NAME>-xxxxxxx.rootfs.wic into SD-Card <br>
## (4)	Use SD-Card Boot Mode to boot in SRG/PICO-IMX8P, SRG/PICO-IMX8PL
