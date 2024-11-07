This README file contains information on the contents of the meta-aaeon-imx8p layer.

Please see the corresponding sections below for details.


=================================================
Build SRG-IMX8P BSP
(1)	Download Yocto BSP with kernel 5.15.71
   $ mkdir imx-yocto-bsp 
   $ cd imx-yocto-bsp 
   $ repo init -u https://github.com/aaeonaeu/aaeon-nxpimx8-manifest -b main  -m aaeon-kirkstone-v01.xml
   $ repo sync
(2)	Environment setup
   $ DISTRO=fsl-imx-wayland MACHINE=imx8mpevk source aaeon-imx-setup-release.sh -b imx8p_build
(3)	Build NXP IMX BSP
   $ bitbake imx-image-full
=================================================
Note:
(1)	If FetchError,then change git branch=master => branch=main
=================================================


