# Goldenmotor Protocol
This repository contains infornation about data exchange protocol of Goldenmotor's VECxxx family controllers and PI-800 program.

## Navigation

1. [Intro](#intro)
2. [Protocol description](#protocol_description)
    1. [Data packets](#data_packets)



## Intro <a name="intro"></a>
Many times I was tring to get info from Goldenmotor's managers about possibility connection their controller to external systems, but every time they told me that it's not possible and no aswer me any more. But if we look at their site we can find info "Controller has UART, CAN, Bluetooth interfaces". So I decided to conduct a study and present some results here.

In this repo you will find information about connection protocol and C code examples from PI-800 program.

If you have additional information about Goldenmotor's controllers and you want to help this project, please contact me or make push request.


##Protocol description <a name="protocol_description"></a>

###Data packets <a name="data_packets"></a>
All data packets in this protocol has length <= 256 bytes

| START | CMD    | STATUS | Payload       | CRC    |
|-------|--------|:------:|---------------|--------|
| 0x66  | 1 byte | 1 byte |0 .. 32 bytes  | 1 byte |

**START**
Start byte **0x66**.
All packets start with this byte.

**CMD**
Command type.
For more info see [list of commands](https://github.com/SunnyWolf/goldenmotor_protocol/blob/master/commands.md) page.

**STATUS**
This fild indicates that there are errors in a data packet

> 0x00 - Default value
> 0x20 - OK, packet received successfully

If there's any error in data, controller sends response with STATUS != 0x20

**Payload**
According code that I find in PI-800, data payload length can be 0 or 32 bytes.
If payload length = 0, controller request for data or check connection.
If payload length = 32, controller send data to controller.
Payload contain items with 2 bytes length, first byte is High, second is Low

|   1   |   2   |
|-------|-------|
| HByte | LByte |

**CRC**
Data verification value (1 byte)
The value is calculated by summing all bytes in data packet and takes low byte.