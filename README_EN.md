# ROH Firmware

Firmware, protocol and desktop application for ROH

## Directories

res: resource file for MD files
firmware: firmware update files
OHandSetting: desktop application for WINDOWS

## Files

1. [OHandModBusRTUProtocol_EN.md](OHandModBusRTUProtocol_EN.md) is ModBus-RTU protocol specification
2. roh_registers_v1.h/roh_registers_v1.py: registers definitions for C, C++ & Python
3. ROH-A001-Dexterous Hand-V1.2.pdf: User manual

## Firmware Update

1. Power off the ROHand.
2. Connect USB-485 converter to ROHand and computer.
3. Found your COM port in system device manager
4. Open OHandSetting, Click "Menu"-"File"-"Force Update", choose local source and firmware update file in directory "firmware".
5. Follow the instructions.
6. Done.