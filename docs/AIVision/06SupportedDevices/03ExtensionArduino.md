# Extension - Arduino
## Introduction
The Arduino library is written in C++ and communicates with the K210 AI Vision Sensor via the I²C interface.

Based on this library, users can develop programs that achieve higher efficiency and richer functionalities. This tutorial mainly introduces usage through the `Wire` I²C library, but it is not limited to `Wire`. You may also refer to the `override` example to implement usage in other environments.


## Quick Start
### Hardware Requirements  
| ![](img/A1.png) | ![](img/A2.png) |
| :---: | :---: |
| K210 AI Vision Sensor | Grove to 4-pin Dupont cable |
| ![](img/A3.png) | ![](img/A4.png) |
| Arduino Uno | Expansion Shield |


Software Requirements

| ![](img/A5.png) | ![](img/A6.png) |
| :---: | :---: |
| Arduino IDE | Vision Module API Library |


Refer to the Grove port pin definitions for correct wiring.

### Library Acquisition
You can download the required API library for the vision module from:

#### GitHub：
1. Visit [GitHub](https://github.com/cyc36880/Arduino_k210)
2. Navigate to the Releases section on the lower right.

![](img/A7.png)

3. Download the latest packaged version.

![](img/A8.png)

#### Gitee：
1.Visit [Gitee](https://gitee.com/cyc36880/arduino_k210)  
2. Navigate to the Releases section on the lower right.![](img/A9.png)

3. Download the latest packaged version.

![](img/A10.png)



### Importing the Library in Arduino IDE
Step 1: Open the Arduino IDE, create a new project, go to the Sketch menu, select Include Library → Add .ZIP Library...

![](img/A11.png)

Step 2: Locate the downloaded library file (make sure you have the correct version), select it, and click Open in the bottom-right corner.

![](img/A12.png)

Step 3: In the File menu, navigate to Examples. At the bottom under Examples from Custom Libraries, if you see `ai_camera`, the library has been successfully imported.

![](img/A13.png)

### Examples
The `ai_camera`library provides multiple sample sketches with detailed comments. Together with the API documentation, these examples help you quickly learn how to use the library.

Below is how to open and compile an example to verify your build environment. If the sketch compiles successfully, your environment is set up correctly.



Step 1: The following walkthrough uses 20-Class Object Recognition as the example.

![](img/A14.png)

Step 2: Select the development board and the corresponding COM port currently in use, then click OK.

![](img/A15.png)

Step 3: Click the Verify/Compile button in the upper-left corner. Wait for the compilation to finish. If successful, the output window will display messages as shown in the highlighted box.![](img/A16.png)

Step 4: Click the Upload button in the upper-left corner. The IDE will first compile, then automatically upload the code to the board. Once uploading is complete, a confirmation message will appear in a popup window as shown in the highlighted box.

![](img/A17.png)

Step 5: Demo Effect

+ The K210 Vision Sensor will recognize objects and label them with their names and position information.

![](img/A18.gif)


+ Additionally, print the object name and position information to the serial monitor.

card_id maps to the object names (see the figure on the right).

position contains the bounding box: X, Y, W, H.

![](img/A19.png)![](img/A20)![](img/A21.png)
