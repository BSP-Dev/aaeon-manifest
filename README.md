# AAEON Manifest README (scarthgap)

## Example
- To download the 6.6.23 release
```bash
repo init -u https://github.com/BSP-Dev/aaeon-manifest.git -b scarthgap -m aaeon-scarthgap-v01.xml
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