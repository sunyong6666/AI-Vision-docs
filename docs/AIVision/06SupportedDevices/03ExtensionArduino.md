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


## API
The API is used to operate the Vision Module. Communication with the module follows a specified protocol and requires basic data handling. By using the API, you can abstract away low-level operations and simplify your application logic.

(The examples below take an ESP32 development board as the target platform.)

### Usage Notes  
+ AprilTag generation (Tag Recognition)

You can generate tags using an online [AprilTag generator](https://chaitanyantr.github.io/apriltag.html).

Set Tag Family to`TAG36H11` (this is the family used by the Vision Module).

Set Tag ID as needed (typical range: 0–200).

Print the generated tag and ensure good lighting and focus during testing.

+ QR code generation (QR Code Recognition)

Use any standard QR code generator [website](https://cli.im/) or tool.

Enter the text/content to be encoded and click Generate.

Ensure sufficient print quality and size so the module can decode reliably.

### Class: AiCamera
The AiCamera class is the fundamental object used to operate the AI Vision Module.

#### Constructor
```cpp
class AiCamera(uint8_t addr=0x24)
```

Creates an AiCamera object.

Parameters:

`addr`→ The I²C address of the Vision Module.

Default value: 0x24

Since the AI Vision Module typically uses a single address, the default setting is usually sufficient.

#### Parameter Macros
```cpp
enum Register
{
    AI_CAMERA_COLOR,            // 颜色获取
    AI_CAMERA_PATCH,            // 色块追踪
    AI_CAMERA_TAG,              // 标签识别
    AI_CAMERA_LINE,             // 线条识别
    AI_CAMERA_20_CLASS,         // 20类物体识别
    AI_CAMERA_QRCODE,           // 二维码识别
    AI_CAMERA_FACE_ATTRIBUTE,   // 人脸属性
    AI_CAMERA_FACE_RE,          // 人脸识别
    AI_CAMERA_DEEP_LEARN,       // 深度学习
    AI_CAMERA_CARD,             // 卡片识别
    AI_CAMERA_WIFI_SERVER,      // 无线图传
};
enum Color
{
    AI_CAMERA_COLOR_RED,     // 红色
    AI_CAMERA_COLOR_GREEN,   // 绿色
    AI_CAMERA_COLOR_BLUE,    // 蓝色
    AI_CAMERA_COLOR_YELLOW,  // 黄色
    AI_CAMERA_COLOR_BLACK,   // 黑色
    AI_CAMERA_COLOR_WHITE,   // 白色
};
```

+ These macros are used when switching modes, reading/writing data in a specific mode, and configuring color settings of the Vision Module.

#### Function
##### Init
+ Init(int sda=-1, int scl=-1)

Description:

Initializes the AI Vision Module over the I²C interface.

**Parameters:**

+ sda → I²C data line (SDA).

Default: -1 (use the board’s default SDA pin).

+ scl → I²C clock line (SCL).

Default: -1 (use the board’s default SCL pin).

**Example:**

```cpp
#include <Arduino.h>   // 引入 Arduino 头文件
#include "ai_camera.h" // 引入 ai视觉模块的库头文件

// 设置 ai 视觉模块操作句柄
AiCamera ai_camrea_handle;


void setup()
{
    ai_camrea_handle.Init();   // 初始化
}

void loop()
{
}
```

##### begin
+ begin(int sda=-1, int scl=-1)

> ##### Use the same style as Init to define this function, in order to keep it consistent with the Arduino coding style.

##### set_sys_mode
+ set_sys_mode(uint8_t mode);

Set the working mode of the AI visual module.

**Parameter ：**

+ mode – Operating Mode
    - Refer to Parameter Macros.

**Example:**

```cpp
#include <Arduino.h>   // 引入 Arduino 头文件
#include "ai_camera.h" // 引入 ai视觉模块的库头文件

// 设置 ai 视觉模块操作句柄
AiCamera ai_camrea_handle;


void setup()
{
    ai_camrea_handle.Init();   // 初始化
    // 设置模式为二维码识别模式
    ai_camrea_handle.set_sys_mode(AI_CAMERA_QRCODE); 
}

void loop()
{
}
```

![](img/A22.gif)

After uploading the code to the development board and resetting the chip, the Vision Module’s operating mode switched from Line Recognition mode to QR Code Recognition mode.

##### get_sys_mode
+ get_sys_mode()

Get the current operating mode of the device

**Return Value:**

AI_CAMERA_COLOR ~ AI_CAMERA_CARD

> Indicates the current mode type. Compare the return value with the Parameter Macros to determine the mode.
>

**Example:**

```cpp
#include <Arduino.h>   // 引入 Arduino 头文件
#include "ai_camera.h" // 引入 ai视觉模块的库头文件

// 设置 ai 视觉模块操作句柄
AiCamera ai_camrea_handle;


void setup()
{
    Serial.begin(115200);      // 初始化串口
    ai_camrea_handle.Init();   // 初始化
}

void loop()
{
    int get_mode = ai_camrea_handle.get_sys_mode(); // 获取系统模式
    if (get_mode == AI_CAMERA_TAG)
    {
        Serial.println("处于标签识别模式");
    }
    else if (get_mode == AI_CAMERA_FACE_ATTRIBUTE)
    {
        Serial.println("处于人脸检测模式");
    }
    else
    {
        Serial.println("其它模式");
    }
    delay(400);
}
```

![](img/A23.gif)

When switching modes using the rotary dial, if you switch to Tag Recognition mode or Face Detection mode, the serial monitor will print the corresponding mode name.


