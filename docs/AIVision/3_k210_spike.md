## Color Sensor Mapping Table in SPIKE Mode

When the port protocol is switched to **SPIKE Mode**, the K210 simulates the output of a SPIKE color sensor. In different vision modes, the meaning of the color value, reflection, and RGB values is as follows:

| Mode                                   | Color Value                           | Reflection      | R Value                | G Value             | B Value             |
| -------------------------------------- | ------------------------------------- | --------------- | ---------------------- | ------------------- | ------------------- |
| Color Detection                        | 9                                     | (R+G+B)/255×100 | Red component          | Green component     | Blue component      |
| Color Block Tracking                   | 9 if found, otherwise -1              | 0               | 0                      | Block X coordinate  | Block Y coordinate  |
| Tag Recognition                        | 9 if found, otherwise -1              | 0               | Tag ID                 | Tag X coordinate    | Tag Y coordinate    |
| Line Detection<br />(bottom line only) | 9 if found, otherwise -1              | 0               | 0                      | Line X coordinate   | Line Y coordinate   |
| 20-Class Object Detection              | 9 if found, otherwise -1              | 0               | Object ID              | Object X coordinate | Object Y coordinate |
| QR Code Recognition                    | 9 if found, otherwise -1              | 0               | 0                      | QR X coordinate     | QR Y coordinate     |
| Face Recognition                       | 9 if learned face found, otherwise -1 | 0               | 0                      | Face X coordinate   | Face Y coordinate   |
| Facial Attribute Detection             | 9 if face found, otherwise -1         | —               | 1=open mouth, 0=closed | 1=smile, 0=neutral  | 1=glasses, 0=none   |
| Deep Learning Recognition              | Learned ID if found, otherwise -1     | —               | —                      | —                   | —                   |
| Road Sign Recognition                  | 9 if found, otherwise -1              | 0               | 0                      | Card X coordinate   | Card Y coordinate   |
| Wi-Fi Streaming Status                 | 1 if connected, otherwise 0           | —               | —                      | —                   | —                   |
| Settings Interface                     | -1                                    | 0               | 0                      | 0                   | 0                   |

> Notes:
>
> * `Color Value` corresponds to the "color code" defined in the SPIKE protocol.
> * `(R+G+B)/255×100` simulates the reflection rate with a result ranging from 0 to 100.
> * Fields not used in certain modes (e.g., Deep Learning, Wi-Fi) may output 0 or random values for RGB.
