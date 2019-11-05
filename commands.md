# List of commands and parameters

## Navigation

1. [Commands](#commands)
    1. [0x02 - Echo](#cmd_2)
    2. [0x10 - Read parameter](#cmd_read)
    3. [0x20 - Write parameter](#cmd_write)
    4. [0x42 - Get LCD info](#cmd_42)

2. [Parameters](#parameters)
    1. [0x01 - Main settings](#param_1)
    2. [0x08 - Protection settings](#param_8)

## Commands <a name="commands"></a>

### 0x02 - Echo <a name="cmd_2"></a>
Check connection

**Send**

|  START |  CMD   | LENGTH |  CRC   |
|:------:|:------:|:------:|:------:|
|  0x66  |  0x02  |  0x00  |  0x68  |

**Response**

|  START |  CMD   | LENGTH |  CRC   |
|:------:|:------:|:------:|:------:|
|  0x66  |  0x02  |  0x00  |  0x68  |

### 0x10 - Read parameter <a name="cmd_read"></a>
This command tells controller send selected parameter values to user.
CMD filed is a sum of READ value and PARAMETER value.

**Example (reading protection settings)**

|  START |  CMD   | LENGTH |  CRC   |
|:------:|:------:|:------:|:------:|
|  0x66  |  0x18  |  0x00  |  0x7E  |

### 0x20 - Write parameter<a name="cmd_write"></a>
This command is sends values of selected parameter to controller.
CMD filed is a sum of WRITE value and PARAMETER value.

**Example**

|  START |  CMD   | LENGTH | PAYLOAD... |  CRC   |
|:------:|:------:|:------:|:----------:|:------:|
|  0x66  |  0x28  | 1 byte |  N bytes   | 1 byte |

### 0x42 - LCD info<a name="cmd_42">

1. BAT <a name="cmd42_BAT">
	
	Type: `UINT8`, offset: `0`
	
	Battery charging value. The value ranges from 0 to 5.
	
2. CUROLD <a name="cmd42_CUROLD">
	
	Type: `UINT8`, offset: `1`
	
3. TSNS <a name=cmd42_TSNS>
	
	Type: `UINT16`, offset: `2`
	
	The time that has elapsed since the last Hall sensor operation.
	
	> RMP = 60000 / TSNS
	
4. STATE <a name="cmd42_STATE">
	
	Type: `UINT8`, offset: `4`
	
	> 0 - No error
	
	> 1 - Stalling protection
	
	> 2 - Throttle protection
	
	> 3 - Hall protection
	
	> 4 - Under-voltage protection
	
	> 5 - MOSFET protection
	
	> 6 - Controller over-heat protection
	
	> 7 - Motor over-current protection
	
	> 8 - Over-voltage protection
	
5. USER <a name="cmd42_USER">
	
	Type: `UINT16`, offset: `5`
	
6. VOL (UINT16)<a name="cmd42_VOL">
	
	Type: `UINT16`, offset: `7`
	
	Battery voltage value multiplied by 10
	
	> voltage = VOL / 10  
	
7. CUR <a name="cmd42_CUR">
	
	Type: `UINT16`, offset: `9`
	
	Current value multiplied by 10
	
	> current = CUR / 10
	
|START | CMD  | LENGTH |[BAT](#cmd42_BAT)|[CUROLD](#cmd42_CUROLD)|[TSNS](#cmd42_TSNS)|[STATE](#cmd42_STATE)|[USER](#cmd42_USER)|[VOL](#cmd42_VOL)|[CUR](#cmd42_CUR)|
|:----:|:----:|:------:|:---------------:|:---------------------:|:-----------------:|:-------------------:|:-----------------:|:------------------:|:---------------:|
| 0x66 | 0x42 |  0x??  |      1 byte     |         1 byte        |      2 bytes      |        1 byte       |      2 bytes      |    2 bytes       |     2 bytes     |
	
## Parameters <a name="parameters"></a>

### 0x01 - Main settings<a name="param_1">
	
1. FLAG - Flags <a name="p1_FLAG">
	
	Type: `UINT8`, offset: `0`
	
	> bit 0: Regeneration braking enable
	
	> bit 1: Reverse enable
	
	> bit 2: Throttle enable
	
	> bit 3: PAS enable
	
2. PAS - PAS ratio <a name="p1_PAS">

	Type: `UINT8`, offset: `1`
	
3. NBV - Nominal battery voltage <a name="p1_NBV">

	Type: `UINT8`, offset: `2`
	
4. OPV - Over-voltage protection value <a name="p1_OPV">
	
	Type: `UINT8`, offset: `3`
	
5. UPV - Under-voltage protection value <a name="p1_UPV">

	Type: `UINT8`, offset: `4`

6. BDC - Battery drawn current <a name="p1_BDC">

	Type: `UINT8`, offset: `5`
	
7. FS - Max forward speed, RPM <a name="p1_FS">

	Type: `UINT16`, offset: `6`

8. EBS - Max EBS phase current <a name="p1_EBS">
	
	Type: `UINT16`, offset: `8`
	
9. ACC - Acceleration, % <a name="p1_ACC">

	Type: `UINT16`, offset: `10`
	
10. RPC - Rated phase current <a name="p1_RPC">
	
	Type: `UINT16`, offset: `22`
	
11. RS - Max reverse speed, RPM <a name="p1_RS">

	Type: `UINT16`, offset: `24`
	
12. PWD - Password <a name="p1_PWD">

	Type: `UINT16`, offset: `26`
	
13. TVS - Throttle valid speed, RPM <a name="p1_TVS">

	Type: `UINT16`, offset: `28`
	
|START | CMD  | LENGTH |[FLAG](#p1_FLAG)|[PAS](#p1_PAS)|[NBV](#p1_NBV)|[OPV](#p1_OPV)|[UPV](#p1_UPV)|[BDC](#p1_BDC)|[FS](#p1_FS)|[EBS](#p1_EBS)|[ACC](#p1_ACC)|    ?    |    ?    |    ?    |    ?    |    ?    |[RPC](#p1_RPC)|[RS](#p1_RS)|[PWD](#p1_PWD)|[TVS](#p1_TVS)|    ?    |   CRC   |
|:----:|:----:|:------:|:--------------:|:------------:|:------------:|:------------:|:------------:|:------------:|:----------:|:------------:|:------------:|:-------:|:-------:|:-------:|:-------:|:-------:|:------------:|:----------:|:------------:|:------------:|:-------:|:-------:|
| 0x66 | 0x10 |  0x20  |     1 byte     |    1 byte    |    1 byte    |    1 byte    |    1 byte    |    1 byte    |  2 bytes   |   2 bytes    |   2 bytes    | 2 bytes | 2 bytes | 2 bytes | 2 bytes | 2 bytes |   2 bytes    |  2 bytes   |   2 bytes    |   2 bytes    | 2 bytes | 2 bytes |


### 0x08 - Protection settings<a name="param_8"></a>

**Settings list:**

1. HEA - Hall electrical angle <a name="p8_HEA"></a>

	Type: `UINT16`, offset: `0`
	
	> 0 - 120
	
	> 1 - 60

2. PAO - Phase angle offset <a name="p8_PAO"></a>

	Type: `UINT16`, offset: `2`
	
	> value = 182.04 * x

	> -180 <= x <= 180

3. PP  - Pole pairs <a name="p8_PP"></a>
	
	Type: `UINT16`, offset: `4`
	
	> 2 <= value <= 28

4. MS  - Motor speed <a name="p8_MS"></a>
	
	Type: `UINT16`, offset: `6`
	
	> 1 <= value <= 10500

5. WD  - Warranty date <a name="p8_WD"></a>
	
	Type: `UINT16`, offset: `8`

6. SV  - Software version <a name="p8_SV"></a>

	Type: `UINT16`, offset: `10`

7. HWP  - High voltaige protection value <a name="p8_HWP"></a>

	Type: `UINT16`, offset: `12`
	
	> value = 228 * x

8. HWPE - High voltaige protection exit value <a name="p8_HWPE"></a>

	Type: `UINT16`, offset: `14`
	
	> value = 228 * x

9. MWV - Minimum work voltage <a name="p8_MWV"></a>

	Type: `UINT16`, offset: `16`
	
	> value = 228 * x

10. LVP - Low voltage protection value <a name="p8_LVP"></a>

	Type: `UINT16`, offset: `18`
	
	> value = 228 * x

11. LVPE - Low voltage protection exit value <a name="p8_LVPE"></a>

	Type: `UINT16`, offset: `20`
	
	> value = 228 * x
	
	> 0.0 < x < 4.0

12. LVT - Low voltage triggering current reducing <a name="p8_LVT"></a>

	Type: `UINT16`, offset: `26`
	
	> value = 228 * x

13. CFG - Config flags: <a name="p8_CFG"></a>

	Type: `UINT16`, offset: `28`
	
	> bit 5:  1
	
	> bit 8:  Clear undervoltage state while throttle off

	> bit 11: Low voltage protection enabled

	> bit 13: Throttle voltage range protection enabled

	> bit 14: Motor overtemperature protection enabled

	> bit 15: Motor blockage protection enabled (always 1)

14. SPT - Stall protection time <a name="p8_SPT"></a>
	
	Type: `UINT16`, offset: `30`
	
	> value = 10 * x

**Sending parameter to controller**

|START | CMD  | LENGTH |[HEA](#p8_HEA)|[PAO](#p8_PAO)|[PP](#p8_PP)|[MS](#p8_MS)|[WD](#p8_WD)|[SV](#p8_SV)|[HWP](#p8_HWP)|
|:----:|:----:|:------:|:------------:|:------------:|:----------:|:----------:|:----------:|:----------:|:------------:|
| 0x66 | 0x28 |  0x20  |   2 bytes    |   2 bytes    |  2 bytes   |  2 bytes   |  2 bytes   |  2 bytes   |   2 bytes    |

|[HWPE](#p8_HWPE)|[MWV](#p8_MWV)|[LVP](#p8_LVP)|[LVPE](#p8_LVPE)|      ?      |[LVT](#p8_LVT)|[CFG](#p8_CFG)|[SPT](#p8_SPT)|   CRC   |
|:--------------:|:------------:|:------------:|:--------------:|:-----------:|:------------:|:------------:|:------------:|:-------:|
|    2 bytes     |   2 bytes    |   2 bytes    |    2 bytes     | 2a 60 01 c8 |   2 bytes    |   2 bytes    |   2 bytes    | 1 byte  |

**Response**

|  START |  CMD   | LENGTH  |  CRC   |
|:------:|:------:|:-------:|:------:|
|  0x66  |  0x28  |  0x00   |  0x8E  |

