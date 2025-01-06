# ROH Firmware

Firmware, protocol and desktop application for Robotic OHand，i.e.，ROH.

## Directories & Files

- *[FAQs](FAQs)*: frequently asked questions
  - [FAQs.md](FAQs/FAQs_EN.md): FAQs in English
- *[firmware](firmware)*: firmware update files
  - files in subdirectory [ModBus-RTU](firmware/ModBus-RTU) are for ModBus-RTU version.
  - files in subdirectory [SerialCtrl](firmware/SerialCtrl) are for SerialCtrl version, can process commands at 90Hz+.
  - files in subdirectory [ModBus-RTU+SerialCtrl](firmware/ModBus-RTU+SerialCtrl) are for mixed version, recommended.
  - [RELEASE_INFO.md](firmware/RELEASE_INFO.md): release information for each firmware version.
- *[OHandSetting](OHandSetting)*: desktop application for Robotic OHand (also for Prosthetic OHand, so it's name is "OHandSetting"). WINDOWS only currently.
  - [OHandSetting-User-Manual-V1.1.pdf](OHandSetting/OHandSetting-User-Manual-V1.1.pdf): User manual for desktop application.
- *[protocol](protocol)*: protocol specification for ModBus-RTU and SerialCtrl versions
  - [OHandModBusRTUProtocol_EN.md](protocol/OHandModBusRTUProtocol_EN.md): ModBus-RTU protocol specification.
  - [OHandSerialProtocol_EN.md](protocol/OHandSerialProtocol_EN.md): SerialCtrl protocol specification.
  - [roh_registers_v1.h](protocol/roh_registers_v1.h)/[roh_registers_v1.py](protocol/roh_registers_v1.py): registers definitions for C, C++ & Python.
- *[res](res)*: resource file for markdown files
- *[UserManual](UserManual)*: user manual for ROH
  - [ROH-A001-Dexterous-Hand-V1.x.x.pdf](UserManual/ROH-A001-Dexterous-Hand-V1.2.2.pdf): User manual

## Firmware Update

1. Power off the Robotic OHand.
2. Connect USB-485 converter to Robotic OHand and computer.
3. Found your COM port in system device manager
4. Open OHandSetting, select your right COM port then click "Menu"-"File"-"Force Update"
5. Choose local source and firmware update file in directory "firmware".
6. Follow the instructions.
7. Done.
