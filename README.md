# AAEON NXP Manifest README
- This repo is dedicated to the NXP IMX-based modules. Here you can find the Yocto BSP recipes for AAEON.
- You can follow the same steps to build your own customized BSP based on your interests.
- For example, for i.MX Linux BSP releases based on Yocto Project `Kirkstone`, the branch is `kirkstone`
## Install the `repo` utility:
- To use this manifest repo, the `repo` tool must be installed first.
```bash
mkdir ~/bin
curl http://commondatastorage.googleapis.com/git-repo-downloads/repo  > ~/bin/repo
chmod a+x ~/bin/repo
PATH=${PATH}:~/bin
```
## Install essential host packages
- Your Build Host must install required packages for the Yocto build. Reference to the section "Build Host Packages" in the document "Yocto Project Quick build".
    - [Build-Host-Packages](https://docs.yoctoproject.org/5.0.3/brief-yoctoprojectqs/index.html#build-host-packages)

## Download the Yocto Project BSP
```plaintext
mkdir <release> && cd <release>
repo init -u https://github.com/BSP-Dev/aaeon-manifest.git -b <branch name> [ -m <release manifest>]
repo sync
```
- Each branch has detailed READMEs describing exact syntax.

- Please see the corresponding sections below for details.

### Support Devices

| Machine     | DDR   |
| ----------- | ----- |
| uCOM-IMX8P  | 2G/4G |
| uCOM-IMX93  |       |
| SRG-IMX8PL  | 2G/4G |
| PICO-IMX8PL | 2G/4G |

### Build SRG/PICO-IMX8PL BSP
- (1)	Download Yocto BSP with kernel 6.6.36
    ```bash!
    $ mkdir imx-yocto-bsp
    $ cd imx-yocto-bsp
    $ repo init -u https://github.com/BSP-Dev/aaeon-manifest.git -b scarthgap -m aaeon-scarthgap-v02.xml
    $ repo sync
    ```
- (2)	Environment setup
    ```bash!
    # SRG/PICO-IMX8PL (2G DDR)
    $ DISTRO=fsl-imx-wayland MACHINE=srg-imx8pl-2g source aaeon-6636-imx-setup-release.sh -b imx8p_build
	
	# SRG/PICO-IMX8PL (4G DDR)
    $ DISTRO=fsl-imx-wayland MACHINE=srg-imx8pl-4g source aaeon-6636-imx-setup-release.sh -b imx8p_build
    
    ```
    
- (3)	Build NXP IMX BSP
    ```bash!
    $ bitbake imx-image-full

    # For quick test
    $ bitbake core-image-minimal
    ```
- Note: (1)	If FetchError,then change git branch=master => branch=main


### Build uCOM-IMX8P BSP
- (1)	Download Yocto BSP with kernel 6.6.36
    ```bash!
    $ mkdir imx-yocto-bsp
    $ cd imx-yocto-bsp
    $ repo init -u https://github.com/BSP-Dev/aaeon-manifest.git -b scarthgap -m aaeon-scarthgap-v02.xml
    $ repo sync
    ```
- (2)	Environment setup
    ```bash!
    # uCOM-IMX8P (2G DDR)
    $ DISTRO=fsl-imx-wayland MACHINE=ucom-imx8p-2g source aaeon-6636-imx-setup-release.sh -b imx8p_build
	
	# uCOM-IMX8P (4G DDR)
    $ DISTRO=fsl-imx-wayland MACHINE=ucom-imx8p-4g source aaeon-6636-imx-setup-release.sh -b imx8p_build
    
    ```
    
- (3)	Build NXP IMX BSP
    ```bash!
    $ bitbake imx-image-full

    # For quick test
    $ bitbake core-image-minimal
    ```
- Note: (1)	If FetchError,then change git branch=master => branch=main

### Build uCOM-IMX93 BSP
- (1)	Download Yocto BSP with kernel 6.6.23
    ```bash!
    repo init -u https://github.com/BSP-Dev/aaeon-manifest.git -b scarthgap -m aaeon-scarthgap-v01.xml
    ```
- (2)	Setup the build folder for a BSP release:
    ```bash!
	[MACHINE=<machine>] [DISTRO=fsl-imx-<backend>] source ./aaeon-setup-release.sh -b bld-<backend>
	<machine>   defaults to `ucom-imx93-v1`
	<backend>   Graphics backend type
    xwayland    Wayland with X11 support - default distro
    wayland     Wayland
    fb          Framebuffer (not supported for mx8)
    ```
	Examples:
	- Setup for Xwayland
	```bash
	MACHINE=ucom-imx93-v1 DISTRO=fsl-imx-xwayland source ./aaeon-setup-release.sh -b bld-xwayland
	```
    
- (3)	Build an image:
    ```bash!
	bitbake <image recipe>
    ```
- Some image recipe:
	| Image Name | Description |
	| -------- | -------- |
	| imx-image-core | core image with basic graphics and no multimedia |
	| imx-image-multimedia | image with multimedia and graphics |
	| imx-image-full | image with multimedia and machine learning and Qt |
