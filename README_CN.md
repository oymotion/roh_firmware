# ROH 固件

ROH 固件、协议文档和桌面端应用

## 目录 & 文件

- *[FAQs](FAQs)*: 常见问题
  - [FAQs_CN.md](FAQs/FAQs_CN.md): 常见问题解答
- *[firmware](firmware)*: 固件升级文件
  - 子目录中 [ROH-A001_ModBus-RTU](firmware/ROH-A001/ModBus-RTU) 和 [ROH-A002_ModBus-RTU](firmware/ROH-A002/ModBus-RTU) 文件分别为ROH-A001和ROH-A002的ModBus-RTU版本。
  - 子目录中 [ROH-A001_SerialCtrl](firmware/ROH-A001/SerialCtrl) 和 [ROH-A002_SerialCtrl](firmware/ROH-A002/SerialCtrl) 文件分别为ROH-A001和ROH-A002的SerialCtrl版本，能处理90Hz+频率的指令。
  - 子目录中 [ROH-A001_ModBus-RTU+SerialCtrl](firmware/ROH-A001/ModBus-RTU+SerialCtrl) 和 [ROH-A002_ModBus-RTU+SerialCtrl](firmware/ROH-A001/ModBus-RTU+SerialCtrl)文件分别为ROH-A001和ROH-A002的双协议混合版本，推荐使用。
  - 子目录中 [ROH-A001_Can](firmware/ROH-A001/Can) 和 [ROH-A002_Can](firmware/ROH-A002/Can) 文件分别为ROH-A001和ROH-A002的Can协议版本。
  - [RELEASE_INFO.md](firmware/RELEASE_INFO.md): 每个固件版本的发布信息。
- *[OHandSetting](OHandSetting)*: 机器人灵巧手(Robotic OHand)的桌面端应用(仿生手Prosthetic OHand也适用，因此命名OHandSetting)，目前支持 Windows 系统 和 Ubuntu 系统。
  - [OHandSetting使用手册-V1.x.pdf](OHandSetting/OHandSetting使用手册-V1.6.pdf): 桌面端应用使用说明。
- *[protocol](protocol)*: Modbus-RTU和SerialCtrl版本的协议格式
  - [OHandModBusRTUProtocol_CN.md](protocol/OHandModBusRTUProtocol_CN.md): ModBus-RTU 协议格式。
  - [OHandSerialProtocol_CN.md](protocol/OHandSerialProtocol_CN.md): SerialCtrl 和 Can 协议格式。
  - [roh_registers_v1.h](protocol/roh_registers_v1.h)/[roh_registers_v1.py](protocol/roh_registers_v1.py): C，C++ & Python的寄存器定义。
- *[res](res)*: markdown文件的资源文件
- *[UserManual](UserManual)*: ROH用户手册
  - [ROH-A001机器人灵巧手-V1.x.x.pdf](UserManual/ROH-A001机器人灵巧手-V1.3.2.pdf): ROH-A001用户手册。
  - [ROH-A002机器人灵巧手-V1.x.x.pdf](UserManual/ROH-A002机器人灵巧手-V1.1.2.pdf): ROH-A002用户手册。

## 固件更新方法

1. 手头断电；
2. 连接好 USB 转 485 模块以及手头，并将 USB 转 485 模块连接到电脑；
3. 设备管理器内查找好 USB 转 485 模块的串口名；
4. 打开 OHandSetting，选择对应串口，点击“菜单”-“文件”-“强制升级”；
5. 选择“本地文件”-firmware 目录下的升级文件；
6. 按提示操作；
7. 完成升级。
