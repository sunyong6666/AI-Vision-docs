# UART
## Serial Parameters
+ **Baud rate: **115200
+ **Data bits:** 8
+ **Parity:** None
+ **Stop bits: **1
+ **Flow control: **None

⚠ Ensure that the host controller (MCU or PC) uses the same configuration for proper communication.

## Data Packet
|  | Header | Packet Size | Data | Checksum |
| :---: | :---: | :---: | :---: | :---: |
| Byte Length | 3 | 2 | 2+ | 1 |
| Content | ICA |  | id+data_size+data |  |
| Description | “ICA”<br/>Fixed 3-byte header | High byte first, low byte next. Indicates the length of all data bytes following this field. | ID refers to command ID; data_size = length of data; data = payload | Calculated checksum |


Note: A single data packet can contain multiple data objects, but the IDs must not be duplicated.

## Communication Process
Communication consists of read and write operations.

After each command is sent, the device returns an ACK response, reflecting the result of the last command.

### Command IDs
| Command | ID | Description |
| :---: | :---: | :---: |
| Notify | 0 | Sent by device; the host only needs to check this command field |
| Write | 1 | 1 byte: target register address |
| Read | 2 | 2 bytes: address + data length |
| *Data | 10 | Used together with Write command; cannot be used alone |


### Response Codes
| Code | Meaning | Notes |
| :---: | :---: | :---: |
| 0 | Success | Command executed successfully |
| 1 | Packet timeout | Packet not received continuously; gap between segments too long |
| 2 | Data too long | Exceeds buffer capacity |
| 4 | Checksum error | Packet received but checksum failed |
| 7 | Invalid command | No such command |
| 8 | Missing data | Incomplete data in received packet |
| 9 | Register error | Invalid or non-existent register |
| 10 | System error | Device not fully booted, or internal error during communication |
| 11 | Data type error | Invalid data type received |


Error Conditions:

+ **Packet Timeout: **Occurs if a full packet is not received continuously. If the sender splits the packet into segments, long gaps between segments will cause this error.
+ **Checksum Error:** Occurs when the packet is received but checksum does not match. This may be caused by coding issues in the sender or interference during transmission.
+ **System Error: **Occurs if the system is not fully booted or an internal fault happens during communication.

* **Partial Header:** If the first 5 bytes (header + packet size) are not completely received, no error is triggered. After timeout, the device will ignore the packet.



### Communication Example
**Host reads a device register value**

Host sends: `49 43 41` `00 05` `02 02 00 01` ` D7`

+ `49 43 41`: Fixed packet header `ICA` 
+ `00 05` ：Packet length
+ `02 02 00 01` ：Command details

       `02`: Command ID = Read

       `02`: Data length

       `00 01`: Read from address `0x00`, size = 1

+ `D7` ：Checksum



Device replies：`49 43 41` `00 07` `00 01 00` `0A 01 02 ` `E2`

+  `49 43 41`: Fixed packet header
+ `00 07` : Packet length
+ `00 01 00` : Response command

       ID = 0 (response)

       Data length = 1

       Data = 0 (status OK)

+ `0A 01 02`: Data command

        ID = 10 (data)

        Data length = 1

         Data = 02

+ `E2`: Checksum

