# Firmware Versions

| Version  | Date       | Comments                                                    |
| -------- | ---------- | ----------------------------------------------------------- |
| V3.0.130 | 2024-08-29 | Initial release                                             |
| V3.0.140 | 2024-09-12 | Fix bug: current can't be read for motor of thumb rotation. |
|          |            | Fix bug: Incorrect current limit value read.                |
| V3.0.168 | 2024-11-13 | Modify CMD_SET_CUSTOM (only for serial control protocol).   |
|          |            | Enable decelerating when around target position.            |
|          |            | Modify range of thumb rotation to [1, 90] in degree.        |
|          |            | Set current threshold to stop motor to 200mA when stuck.    |
