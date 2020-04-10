### dmidecode {#dmidecode}

dmidecode

1\. 最简单的的显示全部dmi信息：

# dmidecode

这样将输出所有的dmi信息，你可能会被一大堆的信息吓坏，通常可以使用下面的方法。

2.更精简的信息显示：

# dmidecode -q

-q(--quite) 只显示必要的信息，这个很管用哦。

3.显示指定类型的信息：

通常我只想查看某类型，比如CPU，内存或者磁盘的信息而不是全部的。这可以使用-t(--type TYPE)来指定信息类型：

# dmidecode -t bios

# dmidecode -t bios, processor (这种方式好像不可以用，必须用下面的数字的方式)

# dmidecode -t 0,4  (显示bios和processor)

服务器到底能扩展到多大的内存？

[root@TJSJHL218-154 setup]# dmidecode -t 16

# dmidecode 2.9

SMBIOS 2.5 present.

Handle 0x1000, DMI type 16, 15 bytes

Physical Memory Array

Location: System Board Or Motherboard

Use: System Memory

Error Correction Type: Multi-bit ECC

Maximum Capacity: 65280 MB

Error Information Handle: Not Provided

Number Of Devices: 8

查看主机型号和CPU型号:

[root@~dmidecode -s system-product-name

PowerEdge 1950

[root@~]# dmidecode -s processor-version

Intel(R) Xeon(R) CPU           E5405  @ 2.00GHz

Intel(R) Xeon(R) CPU           E5405  @ 2.00GHz

比如要查看主板的详细信息:                          

[root@TJSJHL218-154 setup]# dmidecode -t baseboard

# dmidecode 2.9

SMBIOS 2.5 present.

Handle 0x0200, DMI type 2, 9 bytes

Base Board Information

Manufacturer: Dell Inc.

Product Name: 0K649H

Version: A00

Serial Number: ..CN697028790626.

Handle 0x0A00, DMI type 10, 10 bytes

On Board Device 1 Information

Type: Video

Status: Enabled

Description: Embedded ATI ES1000 Video

On Board Device 2 Information

Type: Ethernet

Status: Enabled

Description: Embedded Broadcom 5708 NIC 1

On Board Device 3 Information

Type: Ethernet

Status: Enabled

Description: Embedded Broadcom 5708 NIC 2

[root@TJSJHL218-154 setup]# dmidecode -t 2

# dmidecode 2.9

SMBIOS 2.5 present.

Handle 0x0200, DMI type 2, 9 bytes

Base Board Information

Manufacturer: Dell Inc.

Product Name: 0K649H

Version: A00

Serial Number: ..CN697028790626.

dmidecode支持的数字参数：

Type   Information

----------------------------------------

0   BIOS

1   System

2   Base Board

3   Chassis

4   Processor

5   Memory Controller

6   Memory Module

7   Cache

8   Port Connector

9   System Slots

10   On Board Devices

11   OEM Strings

12   System Configuration Options

13   BIOS Language

14   Group Associations

15   System Event Log

16   Physical Memory Array

17   Memory Device

18   32-bit Memory Error

19   Memory Array Mapped Address

20   Memory Device Mapped Address

21   Built-in Pointing Device

22   Portable Battery

23   System Reset

24   Hardware Security

25   System Power Controls

26   Voltage Probe

27   Cooling Device

28   Temperature Probe

29   Electrical Current Probe

30   Out-of-band Remote Access

31   Boot Integrity Services

32   System Boot

33   64-bit Memory Error

34   Management Device

35   Management Device Component

36   Management Device Threshold Data

37   Memory Channel

38   IPMI Device

39   Power Supply