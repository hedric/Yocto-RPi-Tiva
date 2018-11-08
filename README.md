# Yocto-RPi-Tiva
Building a custom Linux distribution to a target specific task can be done with the Yocto Project. To build a custom Linux distribution on the Raspberry Pi, one needs a bootloader, Linux kernel and a file system and various applications. The Yocto Project is a collaborative effort of the Linux foundation that uses the Openembedded framework and bitbake build engine.

## Building the Yocto Project on a separate host machine:
Using a Virtual Machine running Ubuntu 18.04 LTS
* Clone the poky reposityory: `git clone -b sumo git://git.yoctoproject.org/poky`
* `cd poky`
* Clone BSP meta-layer for Raspberry Pi: `git clone git://git.yoctoproject,org/meta-raspberrypi`
