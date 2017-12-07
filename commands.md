# List of commands

## Navigation

1. [Commands](#commands)
    1. [0x02 - Echo](#cmd_2)
    2. [0x18 - Get protection settings](#cmd_18)
    2. [0x28 - Set protection settings](#cmd_18)

## Commands <a name="commands"></a>

### 0x02 - Echo <a name="cmd_2"></a>
Check connection

**Send**

|  START |  CMD   | STATUS |  CRC   |
|--------|--------|--------|--------|
|  0x66  |  0x02  |  0x00  |  0x68  |

**Response**

|  START |  CMD   | STATUS |  CRC   |
|--------|--------|--------|--------|
|  0x66  |  0x02  |  0x00  |  0x68  |

### 0x18 / 0x28 - Protection settings<a name="cmd_18"></a>

**Settings list:**

1. HEA - Hall electrical angle
    > 0 - 120

    > 1 - 60

2. PAO - Phase angle offset
    > value = 182.04 * x

    > -180 <= x <= 180

3. PP  - Pole pairs
    > 2 <= value <= 28

4. MS  - Motor speed
    > 1 <= value <= 10500

5. WD  - Warranty date
6. SV  - Software version

7. HW  - High voltaige protection value
    > value = 228 * x

8. HWE - High voltaige protection exit value
    > value = 228 * x

9. MWV - Minimum work voltage
    > value = 228 * x

10. LVP - Low voltage protection value
    > value = 228 * x

11. LVPE - Low voltage protection exit value
    > value = 228 * x

    > 0.0 < x < 4.0

12. LVT - Low voltage triggering current reducing
    > value = 228 * x

13. CFG - Config flags:
    > bit 5:  1

    > bit 8:  Clear undervoltage state while throttle off

	> bit 11: Low voltage protection enabled

	> bit 13: Throttle voltage range protection enabled

	> bit 14: Motor overtemperature protection enabled

	> bit 15: Motor blockage protection enabled (always 1)

14. SPT - Stall protection time
	> value = 10 * x

**Send**

|START | CMD  | STATUS | HEA     | PAO     | PP      | MS      | WS      | SV      | HW      |
|------|------|:------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|
| 0x66 | 0x28 |  0x00  | 2 bytes | 2 bytes | 2 bytes | 2 bytes | 2 bytes | 2 bytes | 2 bytes |

| HWE     | MWV     | LVP     | LVPE    |      ?      | LVT     | CFG     | SPT     | CRC     |
|:-------:|:-------:|:-------:|:-------:|:-----------:|:-------:|:-------:|:-------:|:-------:|
| 2 bytes | 2 bytes | 2 bytes | 2 bytes | 2a 60 01 c8 | 2 bytes | 2 bytes | 2 bytes | 1 byte  |

**Response**

|  START |  CMD   | Length  |  CRC   |
|--------|--------|:-------:|--------|
|  0x66  |  0x28  |  0x00   |  0x8E  |

