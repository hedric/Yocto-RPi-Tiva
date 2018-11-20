# Yocto-RPi-Tiva
Building a custom Linux distribution to a target specific task can be done with the Yocto Project. To build a custom Linux distribution on the Raspberry Pi, one needs a bootloader, Linux kernel and a file system and various applications. The Yocto Project is a collaborative effort of the Linux foundation that uses the Openembedded framework and bitbake build engine.

## Building the Yocto Project on a separate host machine:
Using a Virtual Machine running Ubuntu 18.04 LTS
* Clone the poky reposityory: `git clone -b sumo git://git.yoctoproject.org/poky`
* `cd poky`
* Clone BSP meta-layer for Raspberry Pi: `git clone git://git.yoctoproject.org/meta-raspberrypi`
* Initialize build-environment: `source oe-init-build-env`
* Edit the configuration file bblayers.conf: `vim/nano conf/bblayers.conf`
  * add the row `/home/usrname/poky/meta-raspberrypi \` to bblayers.conf
* Edit the configuration file local.conf: `vim/nano conf/local.conf`
  * Edit machine to match your target device, in this case `MACHINE ??= "raspberrypi3"`
* Comment/disable the PACKAGECONFIG's under Qemu configuration
* Under the row CONF_VERSION = "1" add the row `GPU_MEM = "16"`

* In order to get rid of a networking error due to not being able to access https://www.example.com while trying to bitbake the image, the following changes needed to be made to poky/meta-poky/conf/distro/poky.conf (it does obviously not have to be google)

~~~~
# The CONNECTIVITY_CHECK_URI's are used to test whether we can succesfully
# fetch from the network (and warn you if not). To disable the test set
# the variable to be empty.
# Git example url: git://git.yoctoproject.org/yocto-firewall-test;protocol=git;rev=master    
CONNECTIVITY_CHECK_URIS ?= "https://www.google.com/"
~~~~

* Now we are ready to launch a build, execute `bitbake rpi-basic-image`
  * This will build a small image of an embedded GNU Linux distribution for Raspberry Pi3, this can take a long time depending on the hardware of the host machine.
* The SD-card slot on my Dell PC did not `dd` the image properly so i had to use my MacBook Air
~~~~
sudo diskutil umountDisk /dev/disk2
sudo dd if=Dropbox/rpi-basic-image-raspberrypi3.rpi-sdimg of=/dev/disk2 bs=1m
sync
sudo diskutil umountDisk /dev/disk2
~~~~
* To check the disk name of the SD card one can use `diskutil list`


* For the kernel to talk to external hardware it needs a driver which is represented by a kernel module. And that kernel module needs to exist on the system and it needs to be loaded up to the kernel so that you can use that piece of hardware. The available modules are located in
`/lib/modules/kernel/kernel-version/`
