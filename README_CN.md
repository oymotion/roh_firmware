# ROH 固件

ROH 固件、协议文档和桌面端应用

## 目录说明

res: 文档资源文件目录
firmware: 固件升级文件目录
OHandSetting: WINDOWS 版桌面端工具

## 文件说明

1. [OHandModBusRTUProtocol_CN.md](OHandModBusRTUProtocol_CN.md)为中文版 MosBus-RTU 协议文档
2. roh_registers_v1.h/roh_registers_v1.py: C，C++和 Python 寄存器定义
3. ROH-A001机器人灵巧手-V1.2.pdf: 用户手册
4. OHandSetting使用手册.pdf: 桌面端工具使用手册

## 固件更新方法

1. 手头断电；
2. 连接好 USB 转 485 模块以及手头，并将 USB 转 485 模块连接到电脑；
3. 设备管理器内查找好 USB 转 485 模块的串口名；
4. 打开 OHandSetting，点击“菜单”-“文件”-“强制升级”，选择“本地文件”-firmware 目录下的升级文件；
5. 按提示操作；
6. 完成升级。
