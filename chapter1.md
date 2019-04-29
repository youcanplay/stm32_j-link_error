## Error: Flash Download failed – “Cortex-M4”

最近项目使用`STM32F405RG`芯片，刚开始使用ST-Link进行程序仿真和下载，后来程序容量越来越大，发现使用ST-Link下载很慢，每次都要1~2分钟。查阅资料发现，是由于ST-Link的下载速度比较慢。

更换成J-Link之后，一上来就出现`Error: Flash Download failed – “Cortex-M4“`错误。

后来发现，是由于SW总线的速度过快而无法正确下载程序。默认的SW的总线速度如下所示：

![](/img/J-LINK速度.png)

Keil V5报错如下：

![](/img/错误.png)

```
JLink info:
------------
DLL: V5.12e, compiled Apr 29 2016 15:03:58
Firmware: J-Link ARM V8 compiled Jan 31 2018 18:34:52
Hardware: V8.00
S/N : 20080643 
Feature(s) : RDI,FlashDL,FlashBP,JFlash,GDBFull 

* JLink Info: Found SWD-DP with ID 0x2BA01477
* JLink Info: Found SWD-DP with ID 0x2BA01477
* JLink Info: Found Cortex-M4 r0p1, Little endian.
* JLink Info: FPUnit: 6 code (BP) slots and 2 literal slots
* JLink Info: CoreSight components:
* JLink Info: ROMTbl 0 @ E00FF000
* JLink Info: ROMTbl 0 [0]: FFF0F000, CID: B105E00D, PID: 000BB00C SCS
* JLink Info: ROMTbl 0 [1]: FFF02000, CID: B105E00D, PID: 003BB002 DWT
* JLink Info: ROMTbl 0 [2]: FFF03000, CID: B105E00D, PID: 002BB003 FPB
* JLink Info: ROMTbl 0 [3]: FFF01000, CID: B105E00D, PID: 003BB001 ITM
* JLink Info: ROMTbl 0 [4]: FFF41000, CID: B105900D, PID: 000BB9A1 TPIU
* JLink Info: ROMTbl 0 [5]: FFF42000, CID: B105900D, PID: 000BB925 ETM
ROMTableAddr = 0xE00FF000
**JLink Warning: T-bit of XPSR is 0 but should be 1. Changed to 1.

Target info:
------------
Device: STM32F405RGTx
VTarget = 3.287V
State of Pins: 
TCK: 0, TDI: 0, TDO: 0, TMS: 0, TRES: 1, TRST: 0
Hardware-Breakpoints: 6
Software-Breakpoints: 8192
Watchpoints:          4
JTAG speed: 2000 kHz

**JLink Warning: T-bit of XPSR is 0 but should be 1. Changed to 1.
**JLink Warning: T-bit of XPSR is 0 but should be 1. Changed to 1.
Erase Failed!
Error: Flash Download failed  -  "Cortex-M4"
Flash Load finished at 12:17:04
```

修改SW的速度

![](/img/J-LINK速度修改后.png)

```
JLink info:
------------
DLL: V5.12e, compiled Apr 29 2016 15:03:58
Firmware: J-Link ARM V8 compiled Jan 31 2018 18:34:52
Hardware: V8.00
S/N : 20080643 
Feature(s) : RDI,FlashDL,FlashBP,JFlash,GDBFull 

* JLink Info: Found SWD-DP with ID 0x2BA01477
* JLink Info: Found SWD-DP with ID 0x2BA01477
* JLink Info: Found Cortex-M4 r0p1, Little endian.
* JLink Info: FPUnit: 6 code (BP) slots and 2 literal slots
* JLink Info: CoreSight components:
* JLink Info: ROMTbl 0 @ E00FF000
* JLink Info: ROMTbl 0 [0]: FFF0F000, CID: B105E00D, PID: 000BB00C SCS
* JLink Info: ROMTbl 0 [1]: FFF02000, CID: B105E00D, PID: 003BB002 DWT
* JLink Info: ROMTbl 0 [2]: FFF03000, CID: B105E00D, PID: 002BB003 FPB
* JLink Info: ROMTbl 0 [3]: FFF01000, CID: B105E00D, PID: 003BB001 ITM
* JLink Info: ROMTbl 0 [4]: FFF41000, CID: B105900D, PID: 000BB9A1 TPIU
* JLink Info: ROMTbl 0 [5]: FFF42000, CID: B105900D, PID: 000BB925 ETM
ROMTableAddr = 0xE00FF000

Target info:
------------
Device: STM32F405RGTx
VTarget = 3.287V
State of Pins: 
TCK: 0, TDI: 0, TDO: 0, TMS: 0, TRES: 1, TRST: 0
Hardware-Breakpoints: 6
Software-Breakpoints: 8192
Watchpoints:          4
JTAG speed: 1000 kHz

Erase Done.
Programming Done.
Verify OK.
Application running ...
Flash Load finished at 12:20:26
```

修改内容

