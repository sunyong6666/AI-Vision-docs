# SPIKE Compatibility Mode
When the communication protocol is switched to SPIKE mode, the module simulates the output of a SPIKE color sensor. The K210 will generate corresponding color values, reflectance, and RGB data based on the current AI recognition mode.

_***Note: **_When in SPIKE mode, the module must be connected to a SPIKE device. If not connected, the module will not function properly. To reset to factory settings: First, disconnect the power supply. Hold down the button without releasing it while plugging in a Type-C charger. Once the boot screen appears, release the button to restore default configurations.



When the port protocol is switched to SPIKE mode, the K210 simulates the output of a SPIKE color sensor. In different vision modes, the meaning of the color value, reflectance, and RGB values is as follows:

| **Mode** | **Color Value** | **Reflectance** | **R Value** | **G Value  ** | **B Value  ** |
| :---: | :---: | :---: | :---: | :---: | :---: |
| Color Recognition | 9 | (R+G+B)/255 × 100 (range 0–100) | Red component | Green component | Blue component |
| Color Block Tracking | 9 if found, -1 otherwise | 0 | 0 | Block X coordinate | Block Y coordinate |
| Tag Recognition | 9 if found, -1 otherwise | 0 | Tag ID | Tag X coordinate | Tag Y coordinate |
| Line Recognition (lowest detected line only) | 9 if found, -1 otherwise | 0 | 0 | Line X coordinate | Line Y coordinate |
| 20-Class Object Recognition | 9 if found, -1 otherwise | 0 | Object ID | Object X coordinate | Object Y coordinate |
| QR Code Recognition | 9 if found, -1 otherwise | 0 | 0 | QR code X coord. | QR code Y coord. |
| Face Recognition | 9 if a learned face detected, -1 otherwise | 0 | 0 | Face X coord. | Face Y coord. |
| Face Attributes | 9 if found, -1 otherwise | — | 1 = Mouth open / 0 = Closed | 1 = Smile / 0 = Neutral | 1 = Glasses / 0 = None |
| Deep Learning | Returns detected ID if found, otherwise -1 | — | — | — | — |
| Traffic Sign Recognition | 9 if found, -1 otherwise | 0 | Card ID | Card X coord. | Card Y coord. |
| Wi-Fi Transmission | -1 | — | — | — | — |
| Settings | -1 | 0 | 0 | 0 | 0 |


Additional Notes

+ `<font style="background-color:rgba(255, 255, 255, 0.05);">Color Value</font>` corresponds to the color index defined in the SPIKE protocol.
+ Reflectance is calculated as `<font style="background-color:rgba(255, 255, 255, 0.05);">(R+G+B)/255 × 100</font>`, giving a value between 0–100. 
+ For unused fields (e.g., Deep Learning, Wi-Fi), RGB values may be 0 or random.
+ In Line Recognition mode, only the lowest detected line is reported back to SPIKE.

  
 

