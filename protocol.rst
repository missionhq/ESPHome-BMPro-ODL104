ODL104 BLE Protocol
===================

We can subscribe to notifications on the following characteristics:
- ffffff53-4554-4543-2043-414e20424c45  (BMPro & ODL104 registers)
- 5e132001-73a9-4a6a-ae5f-c7bff0bd075e  (SmartTemp registers)
- ffffffff-ffff-5345-5445-432041555448  (authentication)
- FFFFFF42-4C45-2048-4541-525442454154  (heartbeat)


CHARACTERISTIC uuid: "ffffff53-4554-4543-2043-414e20424c45"
===========================================================

Notifications contain a 4x byte header followed by a payload length byte and finally the payload data bytes as...

<Packet Type><Padding><Device ID><Register ID><Payload Length><Payload Bytes>
  
Packet Type   : Either 0x14 or 0x18  (Unsure what the difference is?) 
Padding       : Always ```0x00```
Device ID     : ID of the Payload source typically 0x40 for the BMPro, 0x90 for the ODL104
Register ID   : Register ID 
Payload Length: Data length (N) 
Payload       : N Bytes 

Example Packet...
  18 00 40 62 04 00 00 34 19
This Notification is for the BMPro(0x40) BatteryVoltage(0x62) = 13.337v (0x3419)

Device IDs and Register IDs
---------------------------

Identified so far: The BMPro (ID=0x40) and the ODL104 (ID=0x90)

Registers for Device ID = 0x40 (Source = BMPro)
Reg  Name            Length   Comments 
                 
0x00  Unknown          7 
0x01  Unknown   
0x0C  Unknown          1 
0x0F  Unknown   
0x10  Unknown   
0x11  Unknown   
0x13  Unknown          4 
0x40  Output 1         5   byte[0] = on/off state, byte [1..4] = current (mA) as INT32
0x41  Output 2         5   byte[0] = on/off state, byte [1..4] = current (mA) as INT32
... 
0x47  Output 8         5   byte[0] = on/off state, byte [1..4] = current (mA) as INT32
0x48  Output 9         5   byte[0] = on/off state, byte [1..4] = current (mA) as INT32
0x49  Light 1          5   byte[0] = on/off state, byte [1..4] = current (mA) as INT32
0x4A  Light 2          5   byte[0] = on/off state, byte [1..4] = current (mA) as INT32
0x4B  Light 3          5   byte[0] = on/off state, byte [1..4] = current (mA) as INT32
0x4C  Pump 1           5   byte[0] = on/off state, byte [1..4] = current (mA) as INT32
0x4D  Pump 2           5   byte[0] = on/off state, byte [1..4] = current (mA) as INT32
0x61  Battery Current  4   Current (mA) as INT32
0x62  Battery Voltage  4   Voltage (mV) as INT32
0x63  Aux. Current     4   Auxilliary Current (mA) as INT32
0x64  Solar Current    4   Solar Current (mA) as INT32
0x65  Solar Voltage    4   Solar input Voltage (mV) as INT32
0x66  Battery Percent  4   0..100% as INT32
0x67  Battery Runtime  4   Runtime in (mins) as UINT32
0x68  Unknown          4   Looks like a INT32
0x69  Status           2   BIT status flags...... BYTE[0] bit 1 = ECO mode, BYTE[1] bit0 = ACSupply, Bit1=AuxSupply, Bit3=SolarSupply
0x6A  Battery Capacity 4   As set in the Mobile App. 300,000 = 300Ah as INT32
0x6B  Aux Voltage      4   Aux input Voltage (mV) as INT32
0x6C  Load Current     4   Total output Current Load on the BMPro (mA) as INT32
0x6D  Supply Current   4   Total Supply Current into the BMPro (mA) as INT32
0x6E  Unknown          8 
0x6F  Unknown          8 
0x70  Unknown          1  

Registers for Device ID = 0x90 <u>(Source = ODL104)</u>
Reg  Name            Length   Comments
0x00 Unknown           7
0x01 Unknown
0x0C Unknown           1
0x0F Unknown  
0x10 Unknown  
0x11 Unknown  
0x13 Unknown           4
0x80 Tank Levels       4   1x Byte per tank. Byte value is 0..4 as 0, 25%, 50%, 75%, 100%
0x83 Unknown           5

Writing to Outputs
------------------
To write to an output, we need to send a write command with packet type 0x3E, the Device and Register IDs, and a payload of 1 byte (1 = on, 0 = off). 
To turn on Light 1 we send
  3E 00 40 49 01 01
And to turn it off 
  3E 00 40 49 01 00  

CHARACTERISTIC uuid: "5e132001-73a9-4a6a-ae5f-c7bff0bd075e" 
===========================================================

Notifications contain a 4x byte header followed by a payload length byte and finally the payload data bytes.
It seems to be used for SmartTemp and maybe other Smart devices

Reg  Name            Length   Comments
0x0C SmartTemp        x    Bytes 11-14 for SmartTemp1.. see below |

SmartTemp packet
byte[11] =  SmartTemp 1 RSSI        (possibly signed byte of RSSI in dBm)
byte[12] =  SmartTemp 1 Battery     (possibly LiPo battery voltage - doesn't seem linear?)
byte[13] =  SmartTemp 1 Temperature
byte[14] =  SmartTemp 1 Humidity

CHARACTERISTIC uuid: "ffffffff-ffff-5345-5445-432041555448"
===========================================================

Notification used for basic challenge/response authentication (there is no header/length/payload).

ODL104 sends us a random UINT32 challenge and expects the same value back XORed with 0x43313078. The response is sent back to the same characteristic.
It then sends an ASCII string "Authenticated" (Need to check what it sends on invalid response?).
We need to authenticate in order to continue to receive notifications. Other notifications stop after 1 minute if we don't respond to the challenge.

CHARACTERISTIC uuid: "FFFFFF42-4C45-2048-4541-525442454154"
===========================================================

Notifications containa a 4x byte UINT32 (there is no header/length/payload).
It seems to be used as a heartbeat published every second and increases by 1 each time.
It could be used to detect if the BLE connection is still active.