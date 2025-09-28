## Extension-Micro_Python

## micro:bit Python User Guide
### Interface Overview
Click the link [micro:bit Python Editor](https://python.microbit.org/v/3/project) to enter the online editor. When entering for the first time, the interface looks like this:

![](img/P1.png)

| _**<font style="color:#DF2A3F;">No.</font>**_ | _**<font style="color:#DF2A3F;">Name</font>**_ | _**<font style="color:#DF2A3F;">Function</font>**_ |
| --- | --- | --- |
| ① | Function Panel | Project management |
| ② | Send Code | Send scripts to the connected micro:bit |
| ③ | Code Editor | Edit user code |
| ④ | Save | Save the project as a .hex file to your computer |
| ⑤ | Open | Open a local file |
| ⑥ | Status Display | Show the current status of the micro:bit |




**Language Switching**

**Step 1: Click the gear icon in the lower-left corner and select the Language button.**

![](img/P2.png)



**Step 2: In the pop-up window, select your desired language.**

![](https://cdn.nlark.com/yuque/0/2025/png/50993674/1739093336591-bc52c413-917c-4fe2-b0ee-583122c4aeb4.png)_**<font style="color:#DF2A3F;">  
</font>**_**Note: It is recommended to use micro:bit V2.0 or above. Lower versions have insufficient memory and may not function properly.**

_**<font style="color:#DF2A3F;"></font>**_

### Quick Start
#### Usage Notes:
+ Due to memory limitations, you cannot directly import all library files.
+ Only import the libraries required for your project. Remove unused libraries to optimize memory usage

#### Downloading Files
Visit [GitHub](https://github.com/cyc36880/microbit_micropython_k210.git) to download the Python driver files.

For users in Mainland China, visit [Gitee](https://gitee.com/cyc36880/microbit_python.git).

1. Choose a hosting platform and select the release version (download the latest version if unsure).

| gitee | github |
| --- | --- |
| ![](https://cdn.nlark.com/yuque/0/2025/png/50993674/1757812851447-d61b16c8-2ce2-4f33-957c-1b0fef9393cb.png) | ![](https://cdn.nlark.com/yuque/0/2025/png/50993674/1757812885125-1dc8f19b-bc8a-4221-9c82-d8d784635e8b.png) |


2. Click Download ZIP and wait for the browser to start downloading.

| gitee | github |
| --- | --- |
| ![](https://cdn.nlark.com/yuque/0/2025/png/50993674/1757813092215-9ce736b2-e8ed-477e-8028-c478d71cc4e8.png) | ![](https://cdn.nlark.com/yuque/0/2025/png/50993674/1757813108978-72caa98f-ee3e-4e7d-b1f1-742ee1fc494c.png) |


3. Save the file to your computer.

| gitee | github |
| --- | --- |
| ![](https://cdn.nlark.com/yuque/0/2025/png/50993674/1757813336294-110abedb-c81c-481b-af78-953ce3f23a77.png) | ![](https://cdn.nlark.com/yuque/0/2025/png/50993674/1757813382822-f554e81f-4b49-47c4-b746-0474615b7d50.png) |
| | |


4. Unzip the file and locate the required Python files.

![](https://cdn.nlark.com/yuque/0/2025/png/50993674/1757813529936-c762ee7b-5278-4e67-90a7-e79abfbcf1ee.png)

When using the libraries, you should at least import the following files:

`color.py`、`DC_motor.py`、`iic_base.py`

These files form the foundation for other device driver libraries. Without them, the system will not function properly.



#### Importing Files
##### Single File Import
1. Click the Open button (lower-left or lower-right).

![](https://cdn.nlark.com/yuque/0/2025/png/50993674/1739094773996-b5c32c10-d8d7-49c5-ac16-e58b1750857f.png)



2. Select the file you want to import and click Open.

![](https://cdn.nlark.com/yuque/0/2025/png/50993674/1739094831506-b0650920-f34d-4343-aaec-b6b5e8a55a3b.png)



3. Select the file you want to import and click Open.

![](https://cdn.nlark.com/yuque/0/2025/png/50993674/1739094935023-82c79d79-0ea4-47ea-8f07-dfd026cd3c62.png)



4. A success message appears at the top, and the file is added to the project list.

![](https://cdn.nlark.com/yuque/0/2025/png/50993674/1739095043237-521305a6-06f8-452e-978e-7076f8dce103.png)

##### Multiple File Import
The process is similar to single file import. (See demo GIF for details.)

![](https://cdn.nlark.com/yuque/0/2025/gif/50993674/1739104070051-a4a45588-be66-4153-bccf-fd874bc7cebc.gif)

#### Device Connection
1. Use a microUSB cable to connect the hub and computer.

![](https://cdn.nlark.com/yuque/0/2024/png/43021412/1732954995103-b799cad1-5338-442e-b28e-abbf569fc50c.png)



2. Use a Grove cable to connect the micro:bit with the vision module.

![](https://cdn.nlark.com/yuque/0/2025/jpeg/50993674/1739495577629-9e4594e5-9aae-4d39-ba36-3a4b4ee8438b.jpeg)



#### Downloading Scripts
![](https://cdn.nlark.com/yuque/0/2025/png/50993674/1739516394408-d895ae30-fc25-4d5a-8054-86d0f175bc8e.png)



Ensure that the micro:bit is connected to your computer. Use the default starter code provided on the website, then click the highlighted button to send the code to the controller.

> On the first download, firmware flashing may take longer. Later downloads will be faster.
>



**Default program effect:**

![](https://cdn.nlark.com/yuque/0/2025/gif/50993674/1739512029203-1593fc06-9803-4555-a455-4789d7de6c64.gif)

Display a heart icon.

After 1 second, scroll “Hello”.

Loop continuously.

```python
from microbit import *

import server_motor # 导入库（文件名）

m1 = server_motor.motor(addr = server_motor.LIGHT_RED) # 创建设备对象

while True:
    m1.run(20) # 对象使用
```

These are examples of servo motor usage.

+ Line 1: from microbit import * imports the micro:bit library. Without this library, functions such as sleep cannot be used.
+ Line 3: import server_motor imports the server_motor library.
+ Line 5: Creates a device operation object. All subsequent operations on the corresponding device must be carried out through this object.
+ Line 8: Demonstrates the use of the object.

All device drivers follow a similar usage pattern.



### Examples
#### Six-Way Grayscale Sensor
Follow the library import steps. In addition to the required libraries, import six_gray.py.

![](https://cdn.nlark.com/yuque/0/2025/png/50993674/1739099208174-e82d0e35-36bf-4ef0-b622-9f1b12869e6d.png)



Connect the micro:bit to the six-way grayscale module using a Grove cable.

![](https://cdn.nlark.com/yuque/0/2025/jpeg/50993674/1739496731045-10a6ebf5-95ca-486f-b6f6-1b52041d89cc.jpeg)



Write the following code in the code editor and write it to micro: bit to see the effect.

```python
from microbit import *

import six_gray # 导入六路灰度库

sg = six_gray.six_gray_sensor() # 创建模块操作对象

while True:
    print(sg.gray()) # 打印六路灰度值
    sleep(500)
```

![](https://cdn.nlark.com/yuque/0/2025/gif/50993674/1739496508442-a7b58f5a-19a9-4213-a2ff-27450383bfa9.gif)

#### OLED & Python Libraries
Referring to the library import process, in addition to the libraries that must be imported, you also need to import oled.py libraries. The project files are shown in the figure below

![](https://cdn.nlark.com/yuque/0/2025/png/50993674/1739099797203-c13d2c4e-ba88-4dd7-bc51-5c8ca953a8f5.png)

```python
from microbit import *

import oled  # 导入OLED库

display = oled.oled()  # 创建模块操作对象

count=0
while True:
    count+=1
    if count>20:
        count=0
    display.set_text(0, 0, "hello %d  " % (count)) # 显示字符串
    sleep(400)

```

![](https://cdn.nlark.com/yuque/0/2025/gif/50993674/1739497777836-d02bdb76-bf68-4ffa-a5b8-f1f6891a7bc8.gif)



_*** For the use of other devices, please refer to the API and try it yourself**_

### micro:bit Python Overview:
The Python library includes multiple device drivers. With Python’s flexibility, you can create more versatile, maintainable, and widely applicable code.



Library Files Overview:

| File | Function |
| :---: | :---: |
| color.py | Color definitions |
| ai_camera.py | AI camera library |
| DC_motor.py | DC motor library |
| iic_base.py | IIC driver library |
| joystick.py | Joystick library |
| light_ring.py | LED ring library |
| oled.py | OLED display library |
| recording.py | Recording module library |
| server_motor.py | Servo motor library |
| servors.py | General servo library |
| six_gray.py | Six-way grayscale library |
| ultrasonic.py | Ultrasonic sensor library |




## API Reference
For detailed API documentation of the visual recognition module in the micro:bit Python editor, please refer to the Block Guide – Vision Recognition.

