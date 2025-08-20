# AAEON NXP & MTK Manifest README
- This repo is dedicated to the NXP IMX-based modules & MTK MEDIATEK-based modules.
- Here you can find the Yocto BSP recipes for AAEON.
- You can follow the same steps to build your own customized BSP based on your interests.
- For example, for i.MX Linux BSP releases based on Yocto Project `Scarthgap`, the branch is `scarthgap`

### **Please switch the `git branch` for more informations !**

## Support Machines
|  Machine  |   Kernel  |
|-----------| --------- |
| SRG-IMX8P | Kirkstone |
| SRG-IMX8PL| Kirkstone |
|uCOM-M510  | Kirkstone |
|uCOM-IMX8P | Scarthgap |
|uCOM-IMX93 | Scarthgap |

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