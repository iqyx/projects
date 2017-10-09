uMesh configuration manual
=====================================


Tree-structured CLI
------------------------

uBLoad bootloader and uMesh use CLI (command line interface) with commands and values in a tree structure which resembles common filesystem structure. CLI can be accessed remotely (in uMesh firmware) or locally using TTL-level UART. There are three basic types of output in the CLI:

  - Log output - every line contains single log message which is prepended with date/time information with log message type.

<pre>
[2014-08-20T16:13:46Z] node1 INFO: checking integrity of boot image 'umesh-2.3.1'
[2014-08-20T16:13:48Z] node1 INFO: check ok, booting...
</pre>

  - command output - formatting is determined by commands

<pre>
radio0: UP
  CSMA/CA access method, modulation GMSK, bitrate 50kbit/s
  freq 433790kHz, tx power 0dBm
  tx bytes 452230, rx bytes 789003
  tx packets 38013, rx packets 58223
</pre>

  - command prompt with user input - green command prompt with actual working position (like working directory in classic filesystems) is displayed and user is allowed to type commands
<pre>
node1 /> interface radio print
</pre>


Commands are placed in a tree like:

<pre>
subtree: /
    subtree: interface
        subtree: ethernet
        subtree: radio
            list: radio_list
                item: [0] radio0
                    subtree: radio_item
                        command: print
                item: [1] radio1
                    subtree: radio_item
                        command: print
                item: [2] radio2
                    subtree: radio_item
                        command: print
            command: print
        command: print
    subtree: system
        command: quit
</pre>

Your current tree position is displayed in the command prompt. Following commands are executed from different positions and are equal:

<pre>
node1 /> interface radio print
node1 / interface> radio print
node1 / interface radio> print
</pre>

Each position within the tree can contain multiple different items:

  * another subtrees which allows building of a hierarchy
  * lists with list items (these are used to select any particular item from available set, eg. selecting radio for further configuration)
  * commands (with possible parameters in two forms - 'command param1 param2 param3' or 'command key1=param1 key2=param2')
  * values - these are commonly displayed with 'print' command and can be set using 'key=value' syntax

Example: setting prequency for radio0 and enabling it:

<pre>
node1 /> interface radio radio0 freq=433790 enabled=yes
</pre>

Detailed uMesh cli manual can be found [here](cli.md)


Bootloader
--------------------------

Bootloader serves multiple purposes:

  * it allows loading of user firmware over the serial console in case when no network connectivity is available or when umesh firmware is corrupted and unbootable
  * it starts independent watchdog to handle situations of possible firmware malfunction
  * it checks integrity of loaded firmware (were there any corruptions?)
  * it can flash new firmware to MCU flash memory if it is requested
  * it maintains information about last known working firmware and handles situations when new nonworking firmware is flashed (it flashes back the last known working firmware on next reboot)
  * it allows formatting of flash (with stored firmwares, configuration, logs) and setting flash encryption keys

### first boot ###

There will be several errors displayed during first boot process due to uninitialized flash and configuration. Boot will obviously fail and basic settings will need to me made.

<pre>
uBLoad 0.18 (20140823)
port STM32F401CCU6 @84MHz, 256KB flash, 64KB sram
Initializing logging subsystem...

[-] - INFO: RTC init
[1970-01-01T00:00:01Z] - WARNING: clock is not set or synchronized
[1970-01-01T00:00:01Z] - INFO: SPI init (SPI1, 10.5MHz)
[1970-01-01T00:00:01Z] - INFO: 8MB SPI flash found @SPI1 (FL1, S25FL064)
[1970-01-01T00:00:01Z] - INFO: opening flash filesystem @FL1
[1970-01-01T00:00:01Z] - WARNING: no valid filesystem on @FL1
[1970-01-01T00:00:01Z] - INFO: attempting to load configuration
[1970-01-01T00:00:01Z] - ERROR: no valid configuration found, boot aborted, entering CLI

uBLoad 0.18 (20140823) Tree command line interface
press [tab] to autocomplete, press ? or type 'help' for help

- />
</pre>

### clock setup ###


### flash encryption key generation and formatting ###


### boot image download ###


### boot image selection ###


uMesh boot and serial console
--------------------------------------

<pre>
uBLoad 0.1 (port qNode4, Sep  1 2014 14:07:49)

[2000-01-01T01:13:54Z] INFO: clog initialized
[2000-01-01T01:13:54Z] DEBUG: test: new log message appended

        Press [Enter] to interrupt boot process.......... continuing

[2000-01-01T01:13:56Z] INFO: checking firmware image integrity...

        computing sha-512 hash [################################################################]
        verifying fw header

[2000-01-01T01:13:56Z] INFO: firmware check finished
[2000-01-01T01:13:56Z] INFO: jumping to user application...
early init
        __  __           _           
  _   _|  \/  | ___  ___| |__      Secure mesh network node firmware
 | | | | |\/| |/ _ \/ __| '_ \ 
 | |_| | |  | |  __/\__ \ | | |    version 0.1 (build Sep  1 2014 14:07:49)
  \__,_|_|  |_|\___||___/_| |_|    port qNode4 (ARM-CM4, STM32F401CCU6)

[2000-01-01T01:13:56Z] INFO: radio1 initialization successful
</pre>



