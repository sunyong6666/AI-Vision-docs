# Serial_Communication
## Packet Structure

|           |             Header            |                                                  Length                                                 |                                     Single Data Packet                                     |             ...             | Checksum |
| :-------: | :---------------------------: | :-----------------------------------------------------------------------------------------------------: | :----------------------------------------------------------------------------------------: | :-------------------------: | :------: |
| Byte Size |               3               |                                                    2                                                    |                                             2+                                             |                             |     1    |
|  Content  |              ICA              |                                                                                                         |                                   id + data\_size + data                                   |        Multiple units       | Sumcheck |
|   Notes   | Composed of the 3 bytes "ICA" | High byte first, low byte second. The length refers to the total number of bytes after the length field | `id`: See ID explanation<br />`data_size`: Length of data<br />`data`: Actual data payload | Multiple data units allowed |          |

---

## Data Communication

Communication consists primarily of **read** and **write** operations with the device.

After each command is sent, the device returns an **acknowledgment (ACK)** indicating the result of the previous operation.

---

### Command IDs

| Command |  ID | Description                                                           |
| :-----: | :-: | :-------------------------------------------------------------------- |
|   ACK   |  0  | \*Sent by the device; the host only needs to check this command field |
|  Write  |  1  | 1-byte address indicating the write target                            |
|   Read  |  2  | 2 bytes: composed of address + length                                 |
|  \*Data |  10 | Cannot be used independently; must accompany write command            |

---

### ACK (Response) Information

|              Response Type              | Code |
| :-------------------------------------: | :--: |
|                 Success                 |   0  |
|       Packet Transmission Timeout       |   1  |
| Data too long — exceeds buffer capacity |   2  |
|    Packet received but checksum error   |   4  |
|        Invalid or unknown command       |   7  |
| Missing data segment in received packet |   8  |
|  Invalid register (register not found)  |   9  |
|               System Error              |  10  |
|         Invalid Data Type Format        |  11  |

**Explanation:**

* **Packet Transmission Timeout**

  Triggered when the full packet is not received continuously. If the sender splits a packet into multiple segments and the time between them is too long, this error occurs.

* **Checksum Error After Complete Reception**

  Occurs when the full packet is received, but the computed checksum does not match.

  > Causes: The sender’s checksum logic is incorrect or environmental noise disrupted transmission.

* **System Error**

  Occurs when the system is not fully initialized or an internal error happens during communication.

> \* If the first 5 bytes of the packet (header + length) are not fully received, no error is reported. The device will silently discard the partial packet after a timeout.

---

### Communication Example

**Host Reads Device Register**

* **Host Sends**

`49 43 41` `00 05` `02 02 00 01` ` D7`

`49 43 41`: Fixed header (ASCII for "ICA")
`00 05`: Length (5 bytes of data follow)
`02 02 00 01`: Command ID `02` (read); data length = 2; address = `00`; size = `01`
`D7`: Checksum (sum of all previous bytes)

---

* **Device Responds**

`49 43 41` `00 07` `00 01 00` `0A 01 02 ` `E2`

`49 43 41`: Fixed header
`00 07`: Length (7 bytes of data follow)
`00 01 00`: ID = 0 (ACK); data length = 1; data = 0 (success)
`0A 01 02`: ID = 10 (data command); length = 1; data = 02
`E2`: Checksum
