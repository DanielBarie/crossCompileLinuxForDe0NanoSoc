# crossCompileLinuxForDe0NanoSoc
Inspired by
- https://forum.digikey.com/t/debian-getting-started-with-the-de0-nano-soc-kit/12434 (1)
- https://github.com/ikwzm/FPGA-SoC-Linux
- https://rocketboards.org/foswiki/Documentation/BuildingBootloader
- https://bitlog.it/20170820_building_embedded_linux_for_the_terasic_de10-nano.html
- https://blog.night-shade.org.uk/2013/12/building-a-pure-debian-armhf-rootfs/
- https://wiki.debian.org/Debootstrap (for debbootstrap documentation)

# recommended reading 
- Understanding the device tree: https://www.nxp.com/docs/en/application-note/AN5125.pdf
- Specifically the FPGA region of the device tree: https://www.kernel.org/doc/Documentation/devicetree/bindings/fpga/fpga-region.txt
- And this one for the Altera/Intel SoCs (walkthrough): https://rocketboards.org/foswiki/Documentation/HOWTOCreateADeviceTree

# Prerequisites
- When running in a VM make sure to have at least 16GB of (virtual) disk space
- Make sure to have a swap file (compilation takes a lot of memory, something like ```arm-none-linux-gnueabihf-gcc: fatal error: Killed signal terminated program cc1 
compilation terminated.``` is an indicator for too little memory. Check dmesg). As an alternative you might want to try to reduce the number of parallel compilation processes. 
- install bison, flex, bc, build-essential, libssl-dev, rsync


# steps
- `wget -c https://developer.arm.com/-/media/Files/downloads/gnu-a/10.3-2021.07/binrel/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf.tar.asc` or whatever version is current.

- Rename this file (remove .asc file ending)

- unpack (tar xvf)

- export variable `export CC=` pwd` /gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/bin/arm-none-linux-gnueabihf-`
- check if we call the right compiler: `${CC}gcc --version`
should read along the lines of 
```
arm-none-linux-gnueabihf-gcc (GNU Toolchain for the A-profile Architecture 10.3-2021.07 (arm-10.29)) 10.3.1 20210621
Copyright (C) 2020 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

```

- get uboot (according to (1))
- fix error: https://blog.lexina.in/2021/05/solution-for-multiple-definition-of-yylloc-error-u-boot/ (declare it as external)
- done building uboot

- check for latest long term support kernel: https://www.kernel.org/category/releases.html and https://www.kernel.org/ for the latest .version
- as of the time of writing: 5.10 (until end of 2026)
- get appropriate kernel sources: `git clone --depth 1 -b v5.10 git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git linux-5.10-armv7-fpga` or from  https://github.com/altera-opensource/linux-socfpga
- `cd linux-5.10-armv7-fpga/`
- `git checkout -b linux-5.10-armv7-fpga refs/tags/v5.10`
- get patches from https://github.com/ikwzm/FPGA-SoC-Linux (download master.zip, unpack)
- Apply at least `patch -p1 < ../<dir from previous step>/files/linux-5.4.105-armv7-fpga.diff` (for armv7_fpga_defconfig), won't take you all the way. Finish it by hand (fpga-bridge renaming)
- After all the patching (or not) do `export CROSS_COMPILE=~/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/bin/arm-none-linux-gnueabihf-`
- `make armv7_fpga_defconfig`
- 

# Steps for building the Altera Kernel
- `mkdir altera_kernel`
- `cd altera_kernel`
- At the time of writing, the default version was 5.4 lts. We go for 5.10: `git clone --depth 1 -b socfpga-5.10.50-lts https://github.com/altera-opensource/linux-socfpga.git`
