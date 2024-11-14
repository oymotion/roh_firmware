# ROH 固件

ROH 固件、协议文档和桌面端应用

## 目录说明

- *res*: 文档资源文件目录
- *firmware*: 固件升级文件目录
  - 子目录ModBus-RTU下为ModBus-RTU版固件
  - 子目录SerialCtrl下为串口协议版固件——带复合读写接口，命令处理频率可达60Hz
  - 子目录ModBus-RTU+SerialCtrl下为混合协议版固件——推荐
- *OHandSetting*: WINDOWS 版桌面端工具

## 文件说明

1. [OHandModBusRTUProtocol_CN.md](OHandModBusRTUProtocol_CN.md)为 MosBus-RTU 协议文档
2. [OHandSerialCtrlProtocol_CN.md](OHandSerialCtrlProtocol_CN.md)为 SerialCtrl 协议文档
3. roh_registers_v1.h/roh_registers_v1.py: C，C++和 Python 寄存器定义
4. ROH-A001机器人灵巧手-V1.2.pdf: 用户手册
5. OHandSetting使用手册.pdf: 桌面端工具使用手册

## 固件更新方法

1. 手头断电；
2. 连接好 USB 转 485 模块以及手头，并将 USB 转 485 模块连接到电脑；
3. 设备管理器内查找好 USB 转 485 模块的串口名；
4. 打开 OHandSetting，选择对应串口，点击“菜单”-“文件”-“强制升级”；
5. 选择“本地文件”-firmware 目录下的升级文件；
6. 按提示操作；
7. 完成升级。
