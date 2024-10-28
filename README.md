# ROH 固件/ROH Firmware

ROH 固件、协议文档和桌面端应用
Firmware, protocol and desktop application for ROH

## Directories/目录说明

res: resource file for MD files / 文档资源文件目录
firmware: firmware update files / 固件升级文件目录
OHandSetting: desktop application for WINDOWS / WINDOWS 版桌面端工具

## Files/文件说明

1. [OHandModBusRTUProtocol_CN.md](OHandModBusRTUProtocol_CN.md)为中文版 MosBus-RUT 协议文档
2. [OHandModBusRTUProtocol_EN.md](OHandModBusRTUProtocol_EN.md) is ModBus-RTU protocol specification
3. roh_registers_v1.h/roh_registers_v1.py: C，C++和 Python 寄存器定义 / registers definitions for C, C++ & Python
4. ROH-A001机器人灵巧手-V1.2.pdf / ROH-A001-Dexterous Hand-V1.2.pdf: 用户手册 / User manual

## Firmware Update/固件更新方法

1. Power off the ROHand.
   手头断电；
2. Connect USB-485 converter to ROHand and computer.
   连接好 USB 转 485 模块以及手头，并将 USB 转 485 模块连接到电脑；
3. Found your COM port in system device manager
   设备管理器内查找好 USB 转 485 模块的串口名；
4. Open OHandSetting, Click "Menu"-"File"-"Force Update", choose local source and firmware update file in directory "firmware".
   打开 OHandSetting，点击“菜单”-“文件”-“强制升级”，选择“本地文件”-firmware 目录下的升级文件；
5. Follow the instructions.
   按提示操作；
6. Done.
   完成升级。
