# List of commands and parameters

## Navigation

1. [Commands](#commands)
    1. [0x02 - Echo](#cmd_2)
    2. [0x10 - Read parameter](#cmd_read)
    3. [0x20 - Write parameter](#cmd_write)

2. [Parameters](#parameters)
    1. [0x08 - Protection settings](#param_8)

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

## Parameters <a name="parameters"></a>

### 0x08 - Protection settings<a name="param_8"></a>

**Settings list:**

1. HEA - Hall electrical angle <a name="p8_HEA"></a>
    > 0 - 120

    > 1 - 60

2. PAO - Phase angle offset <a name="p8_PAO"></a>
    > value = 182.04 * x

    > -180 <= x <= 180

3. PP  - Pole pairs <a name="p8_PP"></a>
    > 2 <= value <= 28

4. MS  - Motor speed <a name="p8_MS"></a>
    > 1 <= value <= 10500

5. WD  - Warranty date <a name="p8_WD"></a>
6. SV  - Software version <a name="p8_SV"></a>

7. HWP  - High voltaige protection value <a name="p8_HWP"></a>
    > value = 228 * x

8. HWPE - High voltaige protection exit value <a name="p8_HWPE"></a>
    > value = 228 * x

9. MWV - Minimum work voltage <a name="p8_MWV"></a>
    > value = 228 * x

10. LVP - Low voltage protection value <a name="p8_LVP"></a>
    > value = 228 * x

11. LVPE - Low voltage protection exit value <a name="p8_LVPE"></a>
    > value = 228 * x

    > 0.0 < x < 4.0

12. LVT - Low voltage triggering current reducing <a name="p8_LVT"></a>
    > value = 228 * x

13. CFG - Config flags: <a name="p8_CFG"></a>
    > bit 5:  1

    > bit 8:  Clear undervoltage state while throttle off

	> bit 11: Low voltage protection enabled

	> bit 13: Throttle voltage range protection enabled

	> bit 14: Motor overtemperature protection enabled

	> bit 15: Motor blockage protection enabled (always 1)

14. SPT - Stall protection time <a name="p8_SPT"></a>
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

