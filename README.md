# crossCompileLinuxForDe0NanoSoc
Inspired by
- https://forum.digikey.com/t/debian-getting-started-with-the-de0-nano-soc-kit/12434 (1)
- https://github.com/ikwzm/FPGA-SoC-Linux
- https://rocketboards.org/foswiki/Documentation/BuildingBootloader

# recommended reading 
- Understanding the device tree (we'll be patching): https://www.nxp.com/docs/en/application-note/AN5125.pdf
- Specifically the FPGA region of the device tree: https://www.kernel.org/doc/Documentation/devicetree/bindings/fpga/fpga-region.txt
- And this one for the Altera/Intel SoCs (walkthrough): https://rocketboards.org/foswiki/Documentation/HOWTOCreateADeviceTree

# steps
- `wget -c https://developer.arm.com/-/media/Files/downloads/gnu-a/10.3-2021.07/binrel/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf.tar.asc` or whatever version is current.

- rename this (remove .asc file ending)

- unpack (tar xvf)

- export variable ` export CC=`pwd`/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/bin/arm-none-linux-gnueabihf-`
- check if we call the right compiler: `${CC}gcc --version`
should read along the lines of 
```
arm-none-linux-gnueabihf-gcc (GNU Toolchain for the A-profile Architecture 10.3-2021.07 (arm-10.29)) 10.3.1 20210621
Copyright (C) 2020 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

```
- install bison, flex, bc
- get uboot (according to (1))
- fix error: https://blog.lexina.in/2021/05/solution-for-multiple-definition-of-yylloc-error-u-boot/ (declare it as external)
- done building uboot

- check for latest long term support kernel: https://www.kernel.org/category/releases.html
- as of the time of writing: 5.10 (until end of 2026)
- get appropriate kernel sources: `git clone --depth 1 -b v5.10 git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git linux-5.10-armv7-fpga`
- `cd linux-5.10-armv7-fpga/`
- `git checkout -b linux-5.10-armv7-fpga refs/tags/v5.10`
- 
