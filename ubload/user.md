uBLoad user manual
===============================


Compiling and flashing uBLoad
--------------------------------------------

You need the following to compile uBLoad:

  * recent GCC (arm-none-eabi-).  You can get it at [https://launchpad.net/gcc-arm-embedded](https://launchpad.net/gcc-arm-embedded)
  * python
  * scons

Run the following command:

``` .bash
$ scons PLATFORM=qnode4
scons: Reading SConscript files ...
uBLoad branch master, revision 1f6174a, date 2015-01-28 23:40:16 +0100

Build configuration:
	build date = 2015-01-29 08:53:59 CET
	platform = qnode4
	toolchain = arm-none-eabi

scons: done reading SConscript files.
scons: Building targets ...
make_ver(["platforms/qnode4/version.h"], [])
Compiling platforms/qnode4/config_port.o
Compiling common/cli.o
...
Compiling crypto/sha512.o
Linking ubload_qnode4.elf
arm-none-eabi-size ubload_qnode4.elf
   text	   data	    bss	    dec	    hex	filename
  25428	    112	    284	  25824	   64e0	ubload_qnode4.elf
arm-none-eabi-objcopy -O binary ubload_qnode4.elf ubload_qnode4.bin
scons: done building targets.
...
```

This will create two files - ubload_qnode4.elf and ubload_qnode4.bin. You can flash them using JTAG/SWD or any other method
you are familiar with. SConscript files for some platforms contain everything required for flashing using stlinkv2. You can use:

``` .bash
$ scons PLATFORM=qnode4 program
TODO: paste output here
```




Accessing uBLoad serial console and CLI
------------------------------------------------





This section describes how to access serial console of your board using
a linux computer.

If your bootloader console is connected locally using USB to serial adaptor,
it can be easily accessed using the screen command:

``` .bash
screen /dev/ttyUSB0 115200
```




You can connect your serial port remotely and access it over TCP. It can be easily done
using ser2net. Relevant configuration for ser2net to export /dev/ttyUSB0 on port 5000:

```
5000:raw:120:/dev/ttyUSB0:115200 1STOPBIT 8DATABITS -XONXOFF -RTSCTS LOCAL
```

If your console is operated remotely (using ser2net or socat for example), you can use netcat.
There are probably multiple better options.

``` .bash
stty -icanon -echo; netcat localhost 5000; stty icanon echo
```





After powering the board you will see uBLoad initializing:

```
### uBLoad (umeshFw bootloader), qNode4 platform
### version master, cd7eb49, 2015-01-28 20:31:28 +0100, build date 2015-01-28 21:14:11 CET
INFO: spi_flash: flash detected Spansion S25FL208K, size 1048576 bytes
INFO: sffs: filesystem mounted successfully
INFO: sffs: sectors t=256 e=37 u=1 f=0 d=218 o=0, pages t=3840 e=562 u=1636 o=1642
INFO: sffs: space total 983040 bytes, used 418816 bytes, free 564224 bytes

Press [enter] to interrupt the boot process....................
```

You have to press the return key during the time limit.

```
uBLoad command line interface, type <help> to show available commands.

uBLoad(unknown) > 
```

A simple command line interface will be started. You can now use all available commands to configure your uBLoad parameters
and manipulate firmware images. If the console is kept idle for a configurable amount of time, uBLoad will automatically reset
the board and try to boot again.

If you let it boot, you will see similar output during the first boot without a valid firmware:

```
Press [enter] to interrupt the boot process....................

INFO: fw_image: parsing firmware image...
WARNING: fw_image: unknown section magic 0xffffffff
WARNING: fw_image: unknown section magic 0xffffffff
CRITICAL: fw_image: firmware structure check & parsing failed
CRITICAL: Required firmware verification failed, requesting reset.
```


Example CLI output during firmware upgrade
-----------------------------------------------

```
### uBLoad (umeshFw bootloader), qNode4 platform
### version master, c5bf803, 2015-02-04 00:58:16 +0100, build date 2015-02-04 22:34:40 CET

INFO: clog initialized
INFO: spi_flash: flash detected Spansion S25FL208K, size 1048576 bytes
INFO: sffs: filesystem mounted successfully
INFO: sffs: sectors t=256 e=197 u=3 f=0 d=56 o=0, pages t=3840 e=2974 u=536 o=330
INFO: sffs: space total 983040 bytes, used 137216 bytes, free 845824 bytes
INFO: config: loading saved running configuration

Press [enter] to interrupt the boot process....................

INFO: ubload: new firmware requested, doing current firmware backup...
INFO: fw_image: parsing firmware image...
INFO: fw_image: firmware vector table found at 0x08008400
INFO: fw_image: firmware structure check & parsing OK
progress 100% [#########################################################################]
INFO: Current firmware saved to file backup.fw (size 66168 bytes)
INFO: ubload: programming requested firmware 'test.fw'
progress 100% [#########################################################################]
progress 100% [#########################################################################]
INFO: fw_image programmed firmware from file test.fw (size 66176 bytes)
INFO: fw_image: parsing firmware image...
INFO: fw_image: firmware vector table found at 0x08008400
INFO: fw_image: firmware structure check & parsing OK
INFO: fw_image: verifying firmware integrity...
progress 100% [#########################################################################]
INFO: fw_image: firmware verification OK
INFO: fw_image: authenticating loaded firmware...
INFO: fw_image: firmware authentication OK
INFO: Jumping to user code
```
