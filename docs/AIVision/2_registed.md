[TOC]



# Registers

## Register Description

* All registers are readable and writable (in principle, but avoid writing to registers that shouldn't be written to, as it may trigger unexpected behavior).
* The actual register address is `Function Base Address` + `Register Offset Address`.
* Unless otherwise specified, the default value of all registers upon power-up is `0`.
* Reading or writing to non-existent registers or reading out-of-bounds returns 0.
* For multi-byte numbers, higher bytes come first, lower bytes follow (big-endian).
* Before writing to a register, ensure that the **AI Vision Module** is in the corresponding function page whenever possible.

|     Function     | Base Address |
| :--------------: | :----------: |
| System Settings  |      0       |
| Color Detection  |      15      |
|  Color Tracking  |      30      |
| Tag Recognition  |      45      |
|  Line Detection  |      60      |
| 20-Class Object  |      75      |
|     QR Code      |      90      |
| Face Attributes  |     105      |
| Face Recognition |     120      |
|  Deep Learning   |     135      |
| Card Recognition |     150      |
| Video Streaming  |     165      |
| General Settings |     180      |

---

## Register Table

### System Registers

| Offset |                         Description                          | Length (Bytes) |
| :----: | :----------------------------------------------------------: | :------------: |
|  0x00  |                   **Operating Mode (0–9)**                   |       1        |
|        |                      0: Color Detection                      |                |
|        |                      1: Color Tracking                       |                |
|        |                      2: Tag Recognition                      |                |
|        |                      3: Line Detection                       |                |
|        |                4: 20-Class Object Recognition                |                |
|        |                     5: QR Code Detection                     |                |
|        |                      6: Face Detection                       |                |
|        |                     7: Face Recognition                      |                |
|        |                       8: Deep Learning                       |                |
|        |                 9: Traffic Sign Recognition                  |                |
|        |                    10\: WIFI transmission                    |                |
|  0x01  | **Startup Verification Value**<br />Value: 9876543210123456789<br />All 0s if startup not completed |       19       |
|  0x02  |     **SD Card Presence**<br />0: Not present, 1: Present     |       1        |
|  0x03  | **ESP32S3 Communication Status**<br />0: Abnormal, 1: Normal<br />(Always abnormal in Spike mode) |       1        |

---

### Color Detection

| Offset |           Description            | Length (Bytes) |
| :----: | :------------------------------: | :------------: |
|  0x00  | **Average RGB at screen center** |       3        |
|        |       r, g, b (each 0–255)       |                |

---

### Color Block Tracking

| Offset |                    Description                     | Length (Bytes) |
| :----: | :------------------------------------------------: | :------------: |
|  0x00  |         Target color ID (range: 1 \~ ...)          |       1        |
|  0x01  |  Detection result<br />0: Not found<br />1: Found  |       1        |
|  0x02  | Color block information: x, y, w, h (2 bytes each) |       8        |

> Color ID 1–6 represent: Red, Green, Blue, Yellow, Black, White

---

### Tag Recognition

| Offset |                         Description                          | Length (Bytes) |
| :----: | :----------------------------------------------------------: | :------------: |
|  0x00  |                  Number of tags recognized                   |       1        |
|  0x01  |                     object 0 Information                     |       12       |
|  0x02  |                     object 1 Information                     |       12       |
|  0x03  |                     object 2 Information                     |       12       |
|  0x04  |                     object 3 Information                     |       12       |
|        | Tag data:<br />id (2), rotation (2), x, y, w, h (2 bytes each) |                |

> `rotation`: 0–359 degrees

---

### Line Detection

| Offset |            Description            | Length (Bytes) |
| :----: | :-------------------------------: | :------------: |
|  0x00  | Line detected:<br />0: No, 1: Yes |       1        |
|  0x01  |         Line information          |       8        |
|  0x02  |         Line information          |       8        |
|  0x03  |         Line information          |       8        |
|        |     x, y, w, h (2 bytes each)     |                |

---

### 20-Class Object Recognition

```text
"Airplane", "Bicycle", "Bird", "Boat", "Bottle", "Bus", "Car", "Cat",
"Chair", "Cow", "Dining Table", "Dog", "House", "Motorbike", "Person",
"Plant", "Sheep", "Sofa", "Ship", "TV"
```

| Offset |            Description            | Length (Bytes) |
| :----: | :-------------------------------: | :------------: |
|  0x00  |    Number of objects detected     |       1        |
|  0x01  |       object 0 Information        |       9        |
|  0x02  |       object 1 Information        |       9        |
|  0x03  |       object 2 Information        |       9        |
|  0x04  |       object 3 Information        |       9        |
|        | id (1), x, y, w, h (2 bytes each) |                |

> Object IDs start at 0 and correspond to the list above.

---

### QR Code Detection

| Offset |                 Description                 | Length |
| :----: | :-----------------------------------------: | :----: |
|  0x00  |       QR detected:<br />0: No, 1: Yes       |   1    |
|  0x01  |           QR code content length            |   1    |
|  0x02  | QR code position: x, y, w, h (2 bytes each) |   8    |
|  0x03  |           QR code content (UTF-8)           |  100   |

---

### Face Attributes

| Offset |                         Description                          | Length (Bytes) |
| :----: | :----------------------------------------------------------: | :------------: |
|  0x00  |                   Number of faces detected                   |       1        |
|  0x01  |                     object 0 Information                     |       8        |
|  0x02  |                     object 1 Information                     |       8        |
|  0x03  |                     object 2 Information                     |       8        |
|  0x04  |                     object 3 Information                     |       8        |
|        |                  x, y, w, h (2 bytes each)                   |                |
|  0x05  |                     object 0 Information                     |       4        |
|  0x06  |                     object 1 Information                     |       4        |
|  0x07  |                     object 2 Information                     |       4        |
|  0x08  |                     object 3 Information                     |       4        |
|        | is\_male（Deprecated but retained）, is\_mouth\_open, is\_smile, is\_glasses |                |

---

### Face Recognition

| Offset |             Description              | Length (Bytes) |
| :----: | :----------------------------------: | :------------: |
|  0x00  |       Number of faces detected       |       1        |
|  0x01  | Number of recognized (learned) faces |       1        |
|  0x02  |         object 0 Information         |       9        |
|  0x03  |         object 1 Information         |       9        |
|  0x04  |         object 2 Information         |       9        |
|  0x05  |         object 3 Information         |       9        |
|  0x06  |     Write 1 to initiate learning     |       1        |
|        |  id (1), x, y, w, h (2 bytes each)   |                |

> “Detected” includes all faces with bounding boxes. “Recognized” refers to previously learned faces.

---

### Deep Learning

| Offset |              Description               | Length (Bytes) |
| :----: | :------------------------------------: | :------------: |
|  0x00  |  Learning trigger:<br />0: No, 1: Yes  |       1        |
|  0x01  | Recognition result:<br />0: No, 1: Yes |       1        |
|  0x02  |          Recognized ID (0–1)           |       1        |

---

### Traffic Sign Recognition

| Offset |            Description            | Length (Bytes) |
| :----: | :-------------------------------: | :------------: |
|  0x00  |  Number of cards detected (0–4)   |       1        |
|  0x01  |       object 0 Information        |       9        |
|  0x02  |       object 1 Information        |       9        |
|  0x03  |       object 2 Information        |       9        |
|  0x04  |       object 3 Information        |       9        |
|        | id (1), x, y, w, h (2 bytes each) |                |

> Card ID mapping (0–6):
> Green Light, Turn Left, Stop, Red Light, Turn Right, HornTarget

---

### WIFI Transmission

| Offset |                         Description                         | Length (Bytes) |
| :----: | :---------------------------------------------------------: | :------------: |
|  0x00  |   Wi-Fi SSID:<br />First byte is length, followed by data   |      100       |
|  0x01  | Wi-Fi Password:<br />First byte is length, followed by data |      100       |
|  0x02  |    Wi-Fi IP:<br />First byte is length, followed by data    |       20       |
|  0x03  |      Wi-Fi connection via QR code:<br />0: No, 1: Yes       |       1        |

> If IP is `0.0.0.0` or length is 0, it means not connected.

---

### Settings

| Offset |                         Description                          | Length (Bytes) |
| :----: | :----------------------------------------------------------: | :------------: |
|  0x00  |                 Fill Light Brightness (0–10)                 |       1        |
|  0x01  |            Fill Light Switch:<br />1: On, 0: Off             |       1        |
|  0x02  | <font color=red>**Factory Test Mode (Do not use)**</font><br />Set to 1 to auto-switch modes at next boot<br />Long press to exit |       1        |
|  0x03  | <font color=green>Language (Read-only)</font><br />0: Chinese, 1: English |       1        |
|  0x04  | <font color=green>Main Controller (Read-only)</font><br />0: K210, 1: ESP32 |       1        |

