# ROH Firmware

Firmware & protocol for ROH

## 目录说明

1. modbus：ModBus-RTU版的固件和发布说明，目录下[OHandModBusRTUProtocol.md](modbus/OHandModBusRTUProtocol.md)为MosBus协议文档；
2. serial_protocol：自有协议版的固件和发布说明，目录下[OHandSerialProtocol.md](serial_protocol/OHandSerialProtocol.md)为串行协议文档；对应SDK请移步这里[https://github.com/oymotion/ohand_serial_sdk](https://github.com/oymotion/ohand_serial_sdk)

## 更新方法

1. 手头断电；
2. 连接好USB转485模块以及手头，并将USB转485模块连接到电脑；
3. 设备管理器内查找好USB转485模块的串口名；
4. windows下以命令行方式运行命令

    ```CMD
    update <update_file> -COMx -115200
    ```

    ```TXT
    其中，
    <update_file>为升级文件，带扩展名，例如serial_protocol\oHandSerialRobotic_SpeedControl_1B01_V3.0-59-c129b08.upd，
    COMx即USB转485模块的串口名
    ```

5. 10秒内打开手头电源；
6. 完成升级。
