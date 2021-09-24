# crossCompileLinuxForDe0NanoSoc
Inspired by
- https://forum.digikey.com/t/debian-getting-started-with-the-de0-nano-soc-kit/12434 (1)
- https://github.com/ikwzm/FPGA-SoC-Linux

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
- fix error: https://blog.lexina.in/2021/05/solution-for-multiple-definition-of-yylloc-error-u-boot/
- 
