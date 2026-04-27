** Disclaimer** All of the information below has been gained through trial-and-error so any help in identifying what some of the other packet values or registers are used for would be much appreciated!

### Data Format

The Data received from a BLE notification contains a 4x byte header followed by a payload length byte and finally the payload data bytes.

| Paket Type | 0x00 | Source ID | Register ID | Payload Length | Payload  Data |
  
| Field | Size (Bytes) | Type | Description |
| :--- | :--- | :--- | :--- |
| Packet Type | 1 | UINT8 | Ether 0x14 or 0x18  (Unsure what the difference is?) |
| 0x00 | 1 | UINT8 | Always `0x00` |
| Source ID | 1 | UINT8 | ID of the Payload source (possibly the node ID on CAN bus ?)  |
| Register ID | 1 | UINT8 | Register ID |
| Payload Length | 1 | UINT8 | Data length ($N$) |
| Payload | $N$ | Bytes | Payload |

Example Data Packet...

  180040620400003419

| Value (Hex) | Description |
| :--- | :--- |
| 18 | Packet Type = 0x18 |
| 00 | Padding = 0x00 |
| 40 | Data Source = 0x40 |
| 62 | Register ID = 0x62 |
| 04 | Payload Length = 0x04
| 00003419 | Payload Value = 0x00003419

This Notification is for the BMPro(0x40) BatteryVoltage(0x62) = 13.337v (0x3419)
## Device IDs and Register IDs

So far I have identified 2x data sources; The BMPro (0x40) and the ODL104 (ID=0x90)

#### Device ID = 0x40 <u>(Source = BMPro)</u>

| Register | Name | Data Length | Description | 
| :--- | :--- | :--- | :--- |
| 0x00 | Unknown | 7x Bytes |  |
| 0x01 | Unknown |  |  |
| 0x0C | Unknown | 1x Byte |  |
| 0x0F | Unknown |  |  |
| 0x10 | Unknown |  |  |
| 0x11 | Unknown |  |  |
| 0x13 | Unknown | 4x Bytes |  |
| 0x40 | Output 1 | 5x Bytes | byte[0] = on/off state, byte [1..4] = current (mA) as INT32
| 0x41 | Output 2 | 5x Bytes | byte[0] = on/off state, byte [1..4] = current (mA) as INT32
| ... |  |  | 
| 0x47 | Output 8 | 5x Bytes | byte[0] = on/off state, byte [1..4] = current mA as INT32
| 0x48 | Output 9 | 5x Bytes | byte[0] = on/off state, byte [1..4] = current (mA) as INT32
| 0x49 | Light 1 | 5x Bytes | byte[0] = on/off state, byte [1..4] = current (mA) as INT32
| 0x4A | Light 2 | 5x Bytes | byte[0] = on/off state, byte [1..4] = current (mA) as INT32
| 0x4B | Light 3 | 5x Bytes | byte[0] = on/off state, byte [1..4] = current (mA) as INT32)
| 0x4C | Pump 1 | 5x Bytes | byte[0] = on/off state, byte [1..4] = current (mA) as INT32
| 0x4D | Pump 2 | 5x Bytes | byte[0] = on/off state, byte [1..4] = current (mA) as INT32
| 0x61 | Battery Current | 4x Bytes | Current (mA) as INT32
| 0x62 | Battery Voltage | 4x Bytes | Voltage (mV) as INT32
| 0x63 | Unknown | 4x Bytes | Looks like a UINT32
| 0x64 | Unknown | 4x Bytes | Looks like a UINT32
| 0x65 | Solar Voltage | 4x Bytes | Seems to be Solar input Voltage (mV) but unsure ?? as INT32
| 0x66 | Battery Percent | 4x Bytes | 0..100% as UINT32
| 0x67 | Battery Runtime | 4x Bytes | Runtime in (mins) as UINT32
| 0x68 | Unknown | 4x Bytes | Looks like a UINT32
| 0x69 | Status | 2x Bytes | Indicates supply sources present, and ECO mode... BYTE[0] bit 1 = ECO mode, BYTE[1] bit0 = ACSupply, Bit1=AuxSupply, Bit3=SolarSupply
| 0x6A | Battery Capacity | 4x Bytes | As set in the Mobile App. 300,000 = 300Ah as INT32
| 0x6B | Aux Voltage | 4x Bytes | Seems to be Aux input Voltage (mV) but unsure ?? as INT32
| 0x6C | Load Current | 4x Bytes | Total output Current Load on the BMPro (mA) as INT32
| 0x6D | Supply Current | 4x Bytes | Total Supply Current into the BMPro (mA) as INT32
| 0x6E | Unknown | 8x Bytes | 
| 0x6F | Unknown | 8x Bytes | 
| 0x70 | Unknown | 1x Byte | 

#### Device ID = 0x90 <u>(Source = ODL104)</u>
| Register ID | Name | Data Length | Description | 
| :--- | :--- | :--- | :--- |
| 0x00 | Unknown | 7x Bytes |  |
| 0x01 | Unknown |  |  |
| 0x0C | Unknown | 1x Byte |  |
| 0x0F | Unknown |  |  |
| 0x10 | Unknown |  |  |
| 0x11 | Unknown |  |  |
| 0x13 | Unknown | 4x Bytes |  |
| 0x80 | Tank Levels | 4x Bytes | 1x Byte per tank. Byte value is 0..4 as 0, 25%, 50%, 75%, 100%
| 0x83 | Unknown | 5x Bytes |

