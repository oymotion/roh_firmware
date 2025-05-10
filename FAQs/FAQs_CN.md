# ROHand 灵巧手问答

## 一、电气

### 1.问：ROHand如何接线？

    答：ROHand从手腕处引出一根四芯线，颜色定义为：

| 线色  | 端子定义 |
| :---: | :------: |
| 红色  | 电源24V+ |
| 黑色  | 电源24V- |
| 棕色  | RS485_A  |
| 白色  | RS485_B  |

当主机和灵巧手供电电源不为同一电源时，需要将灵巧手和主机进行共地处理，具体做法为将电源负和USB转485模块的GND短接。
![ROHand_LS接线图](res/wiring_CN.png)
若灵巧手连接其他设备，如机械臂，以睿尔曼RM65机械臂为例(定制版)，测试线颜色定义为：

| 序号  | 端子定义 |
| :---: | :------: |
|   1   | RS485_A  |
|   2   | RS485_B  |
|   3   |    空    |
|   4   |    空    |
|   5   | 电源24V- |
|   6   | 电源24V+ |

接线方式如下图所示：
![ROHand_LSR接线图](res/wiring_realman_CN.png)

### 2.问：ROHand通讯异常、无法升级如何解决？

    答：请确保USB转RS485模块为傲意所提供，按照以下流程进行检测：当灵巧手通讯异常，或在升级固件时提示升级失败，请首先检查线路连接有无接触不良或断开的情况，其次确认供电电源和USB转485模块之间是否做了共地处理，参考上方接线图。若线路连接正确，仍通讯异常，尝试接上120Ω终端电阻，即将所提供的USB转RS485模块的A端口和R端口短接。若仍无法解决，请联系傲意技术支持。

### 3.问: ROHand的工作电压范围？

    答: ROHand工作电压可接受范围在12 ~ 32V，额定功率为48W，额定电压为24V. 使用超过32V的电压请注意不要带电热插拔，否则会造成电子器件损坏。

### 4.问：ROHand的电流参数？

    答：一代版本的ROHand在24V电压下的静态电流为0.12A，空载运动平均电流为0.25A，最大握力抓取电流为1.9A。

### 5.问：为什么在零位时，手指会有持续电流输出，且大拇指旋转无法归零？

    答：由于灵巧手内部采用PID控制算法，当电机位置有内部或者外部原因产生误差时，算法为使其维持在零位，所以在零位时会有持续电流输出，这是正常现象。大拇指旋转无法归零是为了减少侧面手皮堆积产生的应力，设置1°的偏移角度以减小运动范围，也属于正常现象。
    
## 二、软件

### 1.问: 如何编程控制灵巧手？

    答: 基于ModBus-RTU协议，使用*write_registers*指令，写手指位置寄存器*ROH_FINGER_POS_TARGET0 ~ ROH_FINGER_POS_TARGET5*，分别对应大拇指弯曲、食指弯曲、中指弯曲、无名指弯曲、小指弯曲、大拇指旋转，寄存器值为0 ~ 65535，0表示手指完全伸直，65535表示手指完全弯曲，大拇指旋转0为0°侧掌位，65535表示90°对掌位。详情请参考[roh_registers_v1.h](roh_registers_v1.h)/[roh_registers_v1.py](roh_registers_v1.py)，示例代码见附录1。

### 2.问: 如何获取手指角度以及控制手指角度？

    答: 获取手指角度是通过读手指角度寄存器*ROH_FINGER_ANGLE0 ~ ROH_FINGER_ANGLE5*，获取值为-32768 ~ 32767的有符号数，是实际角度\*100之后的值；控制手指角度是通过写手指角度寄存器*ROH_FINGER_ANGLE_TARGET0 ~ ROH_FINGER_ANGLE_TARGET5*，输入值为-32768 ~ 32767的有符号数，是实际角度\*100之后的值。例：欲控制食指弯曲到101.01°，则将10101写入*ROH_FINGER_ANGLE_TARGET1*寄存器，此时读*ROH_FINGER_ANGLE1*寄存器的值为10101，则手指角度为101.01°。手指角度默认为手指第一关节和掌平面的夹角，详细定义请参考[OHandModBusRTUProtocol_CN.md](OHandModBusRTUProtocol_CN.md)协议文档。

### 3.问: 如何获取手指角度范围？

    答: 每个灵巧手的手指角度范围因工艺会有细微差别，因此需要获取实际的角度范围，可通过分别在*ROH_FINGER_POS_TARGET0 ~ ROH_FINGER_POS_TARGET5*寄存器中写入0和65535，对应手指的最大角度和最小角度。此时读取*ROH_FINGER_ANGLE0 ~ ROH_FINGER_POS_ANGLE5*寄存器，获取到的值即为实际的角度上限和下限。代码参考附录2

### 4.问: 如何简单判断灵巧手是否抓取到物体？

    答: 力反馈功能将在下一个版本发布，目前版本是通过*read_holding_registers*指令，读取手指状态寄存器*ROH_FINGER_STATUS0 ~ ROH_FINGER_STATUS5*，判断电机状态码是否为5(STATUS_STUCK)电机堵转，堵转则为抓取到物体。或者通过写电流限制值寄存器*ROH_FINGER_CURRENT_LIMIT0 ~ ROH_FINGER_CURRENT_LIMIT5*，将电流限制值设置为特定值，电机堵转电流超过限制值后停止运行，重新写入目标位置值后会自动重启。

### 5.问：手指运动时为什么会有抖动？

    答: 灵巧手内部采用PID控制算法，抖动是由于PID参数设置不合理导致的，建议用户根据实际使用场景，在OHandSetting目录下的桌面端工具OHandSetting.exe调整PID参数，使灵巧手运动更加平滑。

### 6.问：灵巧手最高波特率和命令处理频率是多少？

    答：ROH-A001灵巧手最高波特率为115200，最高波特率下，用原生的USB转485，可以支持60Hz的命令处理频率，用专用串行控制协议复合指令，可达到90-100左右。下一代灵巧手支持自动波特率，最高波特率可达921600。

### 7.问：灵巧手的控制方式？各个控制方式是怎么转换的？

    答：ROH-A001灵巧手支持位置控制和角度控制。位置控制直接控制电机丝杆位置，分为逻辑位置和绝对位置，绝对位置为经过出厂校正后的电机编码器的数值。逻辑位置则是将绝对位置的范围映射到0-65535后的数值。手指角度控制本质是将输入角度经过公式推导后，转为相应的绝对位置控制电机，反之也可以通过电机丝杆位置推导出手指角度。

### 8.问：灵巧手内部电机控制算法是基于什么参数？

    答：灵巧手内部电机控制是通过基于位置环的PID控制算法，实现对手指的精准控制。同时也能实时检测运行速度和运行电流，电流值可用作判断电机堵转的部分因素。当电机持续堵转过久时，灵巧手会发热，可通过优化控制逻辑来减少发热，策略是检测到堵转时不再发送大于当前手指位置的指令，而是发送小于或等于当前手指位置指令，直到堵转解除，可参考附录3。

## 三、附录

### 1.控制灵巧手示例python代码

```python
import time

from pymodbus import FramerType
from pymodbus.client import ModbusSerialClient
from roh_registers_v1 import *

COM_PORT = 'COM1'
NODE_ID = 2

client = ModbusSerialClient(COM_PORT, FramerType.RTU, 115200)
client.connect()

if __name__ == "__main__":

    # 握拳
    resp = client.write_registers(ROH_FINGER_POS_TARGET1, [65535, 65535, 65535, 65535, 65535], NODE_ID)
    time.sleep(2)
    resp = client.write_registers(ROH_FINGER_POS_TARGET0, 65535, NODE_ID)
    time.sleep(2)

    # 张开
    resp = client.write_registers(ROH_FINGER_POS_TARGET0, 0, NODE_ID)
    time.sleep(2)
    resp = client.write_registers(ROH_FINGER_POS_TARGET1, [0, 0, 0, 0, 0], NODE_ID)
    time.sleep(2)

    # 写手指角度, 写入值为实际值*100
    real_angle = 15.05
    target_angle = round(real_angle * 100)

    if (target_angle < 0) :
        target_angle += 65536

    resp = client.write_registers(ROH_FINGER_ANGLE_TARGET0, [target_angle], NODE_ID)
    time.sleep(2)
    
    # 读手指当前角度, 实际值为输出值/100
    resp = client.read_holding_registers(ROH_FINGER_ANGLE0, 1, NODE_ID)
    current_angle = resp.registers

    if (current_angle > 32767) :
        current_angle -= 65536

    current_angle = current_angle  / 100.0

    print("当前手指角度：", current_angle)
```

### 2.获取灵巧手角度范围示例

```python
import time

from pymodbus import FramerType
from pymodbus.client import ModbusSerialClient
from roh_registers_v1 import *

COM_PORT = 'COM1'
NODE_ID = 2

client = ModbusSerialClient(COM_PORT, FramerType.RTU, 115200)
client.connect()

if __name__ == "__main__":

    # 握拳
    resp = client.write_registers(ROH_FINGER_POS_TARGET1, [65535, 65535, 65535, 65535, 65535], NODE_ID)
    time.sleep(2)
    resp = client.write_registers(ROH_FINGER_POS_TARGET0, 65535, NODE_ID)
    time.sleep(2)

    # 张开
    resp = client.write_registers(ROH_FINGER_POS_TARGET0, 0, NODE_ID)
    time.sleep(2)
    resp = client.write_registers(ROH_FINGER_POS_TARGET1, [0, 0, 0, 0, 0], NODE_ID)
    time.sleep(2)

    # 写手指角度, 写入值为实际值*100
    real_angle = 15.05
    target_angle = round(real_angle * 100)

    if (target_angle < 0) :
        target_angle += 65536

    resp = client.write_registers(ROH_FINGER_ANGLE_TARGET0, [target_angle], NODE_ID)
    time.sleep(2)
    
    # 读手指当前角度, 实际值为输出值/100
    resp = client.read_holding_registers(ROH_FINGER_ANGLE0, 1, NODE_ID)
    current_angle = resp.registers

    if (current_angle > 32767) :
        current_angle -= 65535

    current_angle = current_angle  / 100.0

    print("当前手指角度：", current_angle)
```

### 3.减少灵巧手堵转发热程序示例

```python

import asyncio
import os
import signal
import sys
import time

from pymodbus import FramerType
from pymodbus.client import ModbusSerialClient

from roh_registers_v1 import *

# ROHand 配置
COM_PORT = "COM1"
NODE_ID = 2
NUM_FINGERS = 6

current_dir = os.path.dirname(os.path.realpath(__file__))
parent_dir = os.path.dirname(current_dir)
sys.path.append(parent_dir)


class Application:

    def __init__(self):
        signal.signal(signal.SIGINT, lambda signal, frame: self._signal_handler())
        self.terminated = False

    def _signal_handler(self):
        print("You pressed ctrl-c, exit")
        self.terminated = True

    def write_registers(self, client, address, values, node_id):
        resp = client.write_registers(address, values, node_id)
        if resp.isError():
            print("client.write_registers() returned", resp)
            return False
        else :
            return True

    async def main(self):
        client = ModbusSerialClient(COM_PORT, FramerType.RTU, 115200)
        client.connect()

        self.write_registers(client, ROH_FINGER_SPEED0, [65535, 65535, 65535, 65535, 65535], NODE_ID)

        # 打开所有手指
        self.write_registers(client, ROH_FINGER_POS_TARGET0, [0, 0, 0, 0, 0], NODE_ID)
        time.sleep(1.5)

        # 旋转大拇指至侧掌位
        print("Moving thumb root...")
        status_tmbRoot = client.read_holding_registers(ROH_FINGER_STATUS5, 1, NODE_ID)

        while status_tmbRoot.registers == 5:
            print("Thumb root stuck, retrying...")
            self.write_registers(client, ROH_FINGER_POS_TARGET5, [0], NODE_ID)
            status = client.read_holding_registers(ROH_FINGER_STATUS5, 1, NODE_ID)

        self.write_registers(client, ROH_FINGER_POS_TARGET5, [65535], NODE_ID)
        time.sleep(1.5)

        pos = [65535, 65535, 65535, 65535, 65535]
        pos_copy = pos.copy()

        while not self.terminated:

            status = client.read_holding_registers(ROH_FINGER_STATUS0, 5, NODE_ID)

            for i in range(5):
                if status.registers[i] == 5:
                    print("Finger", i, "is stuck")
                    #如果手指堵转，将当前位置设置为目标位置
                    resp = client.read_holding_registers(ROH_FINGER_POS0 + i, 1, NODE_ID)
                    pos_copy[i] = resp.registers
                else:
                    pos_copy[i] = pos[i]

            self.write_registers(client, ROH_FINGER_POS_TARGET0, pos, NODE_ID)


if __name__ == "__main__":
    app = Application()
    asyncio.run(app.main())
```
