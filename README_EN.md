# ROH Firmware

Firmware, protocol and desktop application for ROH

## Directories

- *res*: resource file for markdown files
- *firmware*: firmware update files
  - files in subdirectory ModBus-RTU are for ModBus-RTU version
  - files in subdirectory SerialCtrl are for SerialCtrl version, can process commands at 90Hz+.
  - files in subdirectory ModBus-RTU+SerialCtrl are for mixed version, recommended.
- *OHandSetting*: desktop application for WINDOWS

## Files

1. [OHandModBusRTUProtocol_EN.md](OHandModBusRTUProtocol_EN.md) is ModBus-RTU protocol specification
2. [OHandSerialCtrlProtocol_CN.md](OHandSerialCtrlProtocol_CN.md) is SerialCtrl protocol specification
3. roh_registers_v1.h/roh_registers_v1.py: registers definitions for C, C++ & Python
4. ROH-A001-Dexterous Hand-V1.2.pdf: User manual
5. OHandSetting使用手册.pdf: User manual in Chinese (English version is coming soon) for OHandSetting desktop application

## Firmware Update

1. Power off the ROHand.
2. Connect USB-485 converter to ROHand and computer.
3. Found your COM port in system device manager
4. Open OHandSetting, select your right COM port then click "Menu"-"File"-"Force Update"
5. Choose local source and firmware update file in directory "firmware".
6. Follow the instructions.
7. Done.
