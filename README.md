# AAEON Manifest README (scarthgap)

# Build uCOM-IMX8P
# Package requirements
```bash!
sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib zstd liblz4-tool \ 
build-essential chrpath socat cpio python3 python3-pip python3-pexpect lz4 \ 
xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint xterm
```
# repo installation (This step may not be needed if it had already existed)
$ mkdir ~/bin  
$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo  
$ chmod a+x \~/bin/repo  
$ export PATH=\~/bin:$PATH  

# Download Yocto BSP with kernel 6.6.36 (scarthgap)
   $ mkdir imx-yocto-bsp <br />
   $ cd imx-yocto-bsp <br />
   $ repo init -u git@github.com:BSP-Dev/aaeon-manifest.git -b scarthgap -m aaeon-scarthgap-v02.xml <br />
   $ repo sync

# Build BSP
   2G DDR <br />
   $ DISTRO=fsl-imx-xwayland MACHINE=ucom-imx8p-2g source aaeon-6636-imx-setup-release.sh -b imx8p_build <br />
   
   4G DDR <br />
   $ DISTRO=fsl-imx-xwayland MACHINE=ucom-imx8p-4g source aaeon-6636-imx-setup-release.sh -b imx8p_build <br />
   
   If you leave the build code environment, enter imx-yocto-bsp again by: <br />
   $ source setup-environment imx8p_build

## Build NXP IMX BSP
   $ bitbake imx-image-full
    
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


# Build uCOM-IMX93
## Example
- To download the 6.6.23 release
```bash
repo init -u git@github.com:BSP-Dev/aaeon-manifest.git -b scarthgap -m aaeon-scarthgap-v01.xml
```
## Setup the build folder for a BSP release:
```
[MACHINE=<machine>] [DISTRO=fsl-imx-<backend>] source ./aaeon-imx-setup-release.sh -b bld-<backend>
<machine>   defaults to `ucom-imx93-v1`
<backend>   Graphics backend type
    xwayland    Wayland with X11 support - default distro
    wayland     Wayland
    fb          Framebuffer (not supported for mx8)
```
Examples:
- Setup for Xwayland
```bash
MACHINE=ucom-imx93-v1 DISTRO=fsl-imx-xwayland source ./aaeon-imx-setup-release.sh -b bld-xwayland
```
## Build an image:
```plaintext
bitbake <image recipe>
```
Some image recipe:
| Image Name | Description |
| -------- | -------- |
| imx-image-core | core image with basic graphics and no multimedia |
| imx-image-multimedia | image with multimedia and graphics |
| imx-image-full | image with multimedia and machine learning and Qt |