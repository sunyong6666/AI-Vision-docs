# IIC
## Device Address
Supports three selectable addresses: `0x24、``0x25、``0x26`

The address can be selected in the Settings interface.

After configuration, the device will automatically restart to apply the new address, preventing conflicts with other I²C devices.

## Registers
### Register Description
+ All registers are read/write (in principle). However, writing to registers that should not be modified may trigger unintended actions.
+ The actual register address =`Function Base Address`+`Register Offset`
+ Unless otherwise specified, registers are initialized to `0` after power-on.
+ Reading/writing non-existent registers or out-of-range values will return 0.
+ Multi-byte numbers are stored in **big-endian format** (high byte first, low byte last).
+ Before writing to registers, ensure that the `AI Vision Sensor` is in the corresponding function interface.

| Function | Base Address |
| :---: | :---: |
| System Settings | 0 |
| Color Detection | 15 |
| Color Block Tracking | 30 |
| Tag Recognition | 45 |
| Line Detection | 60 |
| 20-class Object Rec. | 75 |
| QR Code Recognition | 90 |
| Face Attributes | 105 |
| Face Recognition | 120 |
| Deep Learning | 135 |
| Card Recognition | 150 |
| Image Transmission | 165 |
| Settings | 180 |


### System Registers
#### sys (System Registers)
| Offset | Description | Length (bytes) |
| :---: | :---: | :---: |
| 0x00 | Operating mode (0–9) | 1 |
| | 0: Color Recognition | |
| | 1: Color Block Tracking | |
| | 2: Tag Recognition | |
| | 3: Line Detection | |
| | 4: 20-class Object Recognition | |
| | 5: QR Code Recognition | |
| | 6: Face Detection | |
| | 7: Face Recognition	 | |
| | 8: Deep Learning	 | |
| | 9: Card Recognition | |
| | 10: Wi-Fi Image Transmission | |
| 0x01 | Boot verification (value: 9876543210123456789; returns 0 if not fully booted) | 19 |
| 0x02 | SD card detection (0: none, 1: detected) | 1 |
| 0x03 | ESP32-S3 communication status (0: error, 1: normal) | 1 |


### get_color (Color Detection)
| Offset | Description | Length (bytes) |
| :---: | :---: | :---: |
| 0x00 | Average RGB color at screen center | 3 (r,g,b; each 0–255) |
| | | |


### find_color (Color Block Tracking)
| Offset | Description | Length (bytes) |
| :---: | :---: | :---: |
| 0x00 | The ID of the color to be tracked. Range: 1 ~ 6 | 1 |
| | | |
| 0x01 | Indicates whether the target color block is detected. 0 = Not found, 1 = Found | 1 |
| | | |
| 0x02 | Provides detailed information of the detected color block. x, y, w, h (2 bytes each) | 8 |
| | | |


> id 1~6
>
> Red, green, blue, yellow, black and white
>



### find_tag (Tag Recognition)
| Offset | Description | Length (bytes) |
| :---: | :---: | :---: |
| 0x00 | Number of tags detected | 1 |
| 0x01 | Reserved (tag count) | 12 |
| 0x02 | Reserved (tag count) | 12 |
| 0x03 | Reserved (tag count) | 12 |
| 0x04 | Tag information<br/>id (2), rotation (2), x, y, w, h (each 2 bytes) | 12 |


> rotation: 0 ~ 359
>

### find_line (Line Recognition)
| Offset | Description | Length (bytes) |
| :---: | :---: | :---: |
| 0x00 | Line detection flag<br/>0 = Not detected, 1 = Detected | 1 |
| | | |
| 0x01 | Line information<br/>x, y, w, h (each 2 bytes) | 8 |
| 0x02 | Line information<br/>x, y, w, h (each 2 bytes) | 8 |
| 0x03 | Line information<br/>x, y, w, h (each 2 bytes) | 8 |


### find_20_class (20-class Object Detection)
```plain
0: Airplane
1: Bicycle
2: Bird
3: Boat
4: Bottle
5: Bus
6: Car
7: Cat
8: Chair
9: Cow
10: Dining Table
11: Dog
12: House
13: Motorcycle
14: Person
15: Potted Plant
16: Sheep
17: Sofa
18: Train
19: TV Monitor
```

| Offset | Description | Length (bytes) |
| :---: | :---: | :---: |
| 0x00 | Number of objects detected | 1 |
| 0x01 | Object data | 9 |
| 0x02 | Object data | 9 |
| 0x03 | Object data | 9 |
| 0x04 | Object information<br/>id (1), x, y, w, h (each 2 bytes) | 9 |


> ID starts from 0, corresponding to the following objects in order:
>

### find_qrcode (QR Code Recognition)
| Offset | Description | Length (bytes) |
| :---: | :---: | :---: |
| 0x00 | QR code detection flag<br/>0 = Not detected, 1 = Detected | 1 |
| | | |
| 0x01 | QR code data length | 1 |
| 0x02 | QR code bounding box<br/>x, y, w, h (each 2 bytes) | 8 |
| 0x03 | QR code information | 100 |


### find_face (Face Attributes)
| Offset | Description | Length (bytes) |
| :---: | :---: | :---: |
| 0x00 | Number of faces detected | 1 |
| 0x01 |  Face 1  x, y, w, h (each 2 bytes) | 8 |
| 0x02 |  Face 2  x, y, w, h (each 2 bytes) | 8 |
| 0x03 |  Face 3  x, y, w, h (each 2 bytes) | 8 |
| 0x04 |  Face 4  x, y, w, h (each 2 bytes) | 8 |
| 0x05 | Face 1 attributes<br/>is_male(deprecated but reserved), is_mouth_open, is_smail, is_glasses | 4 |
| 0x06 | Face 2 attributes<br/>is_male(deprecated but reserved), is_mouth_open, is_smail, is_glasses | 4 |
| 0x07 | Face 3 attributes<br/>is_male(deprecated but reserved), is_mouth_open, is_smail, is_glasses | 4 |
| 0x08 | Face 4 attributes<br/>is_male(deprecated but reserved), is_mouth_open, is_smail, is_glasses | 4 |


### find_face_recognition (Face Recognition)
| Offset | Description | Length (bytes) |
| :---: | :---: | :---: |
| 0x00 | Number of faces detected | 1 |
| 0x01 | Number of faces recognized | 1 |
| 0x02 | Face 1 info    id (1), x, y, w, h (each 2 bytes)   | 9 |
| 0x03 | Face 2 info    id (1), x, y, w, h (each 2 bytes)   | 9 |
| 0x04 | Face 3 info    id (1), x, y, w, h (each 2 bytes)   | 9 |
| 0x05 | Face 4 info    id (1), x, y, w, h (each 2 bytes)   | 9 |
| 0x06 | Learning control<br/>Write 1 = start learning | 1 |


> Detection count: The total number of faces framed on the screen.
>
> Recognition count: The number of faces successfully matched from the learned dataset.
>

### self_lenrning (Deep Learning)
| Offset | Description | Length (bytes) |
| :---: | :---: | :---: |
| 0x00 | (Deep Learning)<br/>0 = No, 1 = Yes | 1 |
| 0x01 | Detection flag<br/>0 = No, 1 = Yes | 1 |
| 0x02 | ID of recognized object (0–1) | 1 |


### find_card (Traffic Sign Recognition)
| Offset | Description | Length (bytes) |
| :---: | :---: | :---: |
| 0x00 | Number of cards detected | 1 |
| 0x01 | Card 1 info<br/>id (1), x, y, w, h (each 2 bytes) | 9 |
| 0x02 | Card 2 info<br/>id (1), x, y, w, h (each 2 bytes) | 9 |
| 0x03 | Card 3 info<br/>id (1), x, y, w, h (each 2 bytes) | 9 |
| 0x04 | Card 4 info<br/>id (1), x, y, w, h (each 2 bytes) | 9 |


Note：ID Mapping (0–6):

0: Green Light; 1: Left Turn; 2: Stop; 3: Red Light; 4: Right Turn; 5: Horn; 6: Target

### wifi_server WIFI (Wi-Fi Transmission)
| Offset | Description | Length (bytes) |
| :---: | :---: | :---: |
| 0x00 | wifi ssid<br/>The first byte indicates the length of the SSID, followed by the SSID string | 100 |
| | | |
| 0x01 | wifi password<br/>The first byte indicates the length of the password, followed by the password string | 100 |
| | | |
| 0x02 | wifi ip<br/>The first byte indicates the length of the IP, followed by the IP address | 20 |
| | | |
| 0x03 | Connect via QR Code<br/>0 = No, 1 = Yes | 1 |


> If the IP address is 0.0.0.0 or the IP length is 0, it indicates that the module is not connected to Wi-Fi.
>

### setting 
| Offset | Description | Length (bytes) |
| :---: | :---: | :---: |
| 0x00 | Fill light brightness<br/>Range: 0–10 | 1 |
| 0x01 | Fill light switch<br/>0 = Off, 1 = On | 1 |
| 0x02 | <font style="color:red;">Factory test (do not use)</font>   When set to 1, the module will automatically cycle through modes on the next startup; long press to exit test mode | 1 |
| 0x03 | <font style="color:green;">Language (read-only)</font>   0 = Chinese, 1 = English | 1 |
| 0x04 | <font style="color:green;">Control chip (read-only)</font>   0 = K210, 1 = ESP32 | 1 |


