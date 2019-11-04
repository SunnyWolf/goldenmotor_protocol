# Goldenmotor Protocol
This repository contains infornation about data exchange protocol of Goldenmotor's VECxxx family controllers and PI-800 program.

## Navigation

1. [Intro](#intro)
2. [Protocol description](#protocol_description)
3. [Commands](https://github.com/SunnyWolf/goldenmotor_protocol/blob/master/commands.md)



## Intro <a name="intro"></a>
Many times I was trying to get info from Goldenmotor's managers about possibility of connection  between their controller and external systems, but every time they told me that it's not possible and didn't reply me any more. But their site contains following info: "Controller has UART, CAN, Bluetooth interfaces". So I decided to conduct a study and present some results here.

In this repo you will find information about connection protocol and C code examples from PI-800 program.

If you have additional information about Goldenmotor's controllers and you want to help this project, please contact me or make pull request.


## Protocol description <a name="protocol_description"></a>

### Data packets <a name="data_packets"></a>
All data packets in this protocol has length <= 256 bytes.

| START |  CMD   | LENGTH |   PAYLOAD     |  CRC   |
|:-----:|:------:|:------:|:-------------:|:------:|
| 0x66  | 1 byte | 1 byte |0 .. 32 bytes  | 1 byte |

**START**

Start byte **0x66**.
All packets start with this byte.

**CMD**

Command type.
For more info see [list of commands](https://github.com/SunnyWolf/goldenmotor_protocol/blob/master/commands.md) page.

**Length**

This fild indicates payload length in bytes.

**Payload**

According code that I found in PI-800, data payload length can be 0 or 32 bytes.
If payload length = 0, controller request for data or check connection.
If payload length > 0, controller send data to user.
Mainly payload contains items with 1 or 2 bytes length.
Byte order is little-endian.

|   1   |   2   |
|-------|-------|
| HByte | LByte |

**CRC**

Data verification value (1 byte).
The value is calculated by summing all bytes in data packet and takes low byte.
```c
uint8_t crc = 0;
for (int i = 0; i < packetLen - 1; i++) {
	crc = (uint8_t) (crc + packetBuffer[i]);
}
```

