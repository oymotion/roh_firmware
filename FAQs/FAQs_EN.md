# ROHand FAQ

## Hardware

### 1. Q: How do I connect ROHand?

**A:** ROHand uses a four-core cable from the wrist with this color coding:

| Color | Definition    |
| :---: | :------------ |
|  Red  | Power 24 V+   |
| Black | Power 24 V-   |
| Brown | RS485_A/CAN_L |
| White | RS485_B/CAN_H |

**Important:** When using separate power supplies for host and ROHand, establish common ground by shorting the power supply negative to the USB-to-485 module's GND.

![ROHand_LS wiring](res/wiring_EN.png)

**Connection to robotic arms (e.g., RM65):**

|  Pin  | Color | Definition    |
| :---: | :---: | :------------ |
|   1   | Brown | RS485_A       |
|   2   | White | RS485_B       |
|   3   |   -   | Not connected |
|   4   |   -   | Not connected |
|   5   | Black | Power 24 V-   |
|   6   |  Red  | Power 24 V+   |

Wiring diagram:  
![ROHand_LSR wiring](res/wiring_realman_EN.png)

---

### 2. Q: How do I troubleshoot ROHand communication issues or failed firmware updates?

**A:** Follow these steps:

1. Confirm that the communication interface is the same. Do not use RS485 to communicate with the CAN version of the ROHand.
2. Ensure that the USB-to-RS485 module or USB-to-PCAN module provided by OYMotion is used.
3. Check for loose or damaged wiring. When using quick-connect terminals, ensure the metal part of the cable makes proper contact, or use a crimping tool to securely attach terminals to the cable.
   ![wiring terminal](res/wiring_terminal.png)
4. Verify proper common grounding (refer to wiring diagram)
5. If issues persist, connect the 120Ω termination resistor by shorting A and R ports(port 2 and port 3). As shown in the following figure:
   ![120Ω resistor](res/terminal_resistor.png)
6. Contact technical support if problems continue

---

### 3. Q: What is ROHand's operating voltage range?

**A:** ROHand operates at 12-24 V DC with these specifications:

- Rated voltage: 24 V
- Rated power: 48 W  
**Note:** The buzzer pulses when voltage drops below required levels.

---

### 4. Q: What are ROHand's current specifications?

**A:** At 24 V:

- Standby current: ~0.12 A
- No-load motion current: ~0.25 A
- Peak 5-finger gripping current: ~2.0 A

---

### 5. Q: Why is there continuous current at the zero position, and why won't the thumb rotation zero completely?

**A:** This is normal behavior:

- PID control maintains position accuracy at zero
- Thumb has a deliberate 1° offset to reduce skin stress

---

### 6. Q: Why do fingers extend fully during self-test?

**A:** The startup self-test extends all fingers. If fingers don't extend:

1. Check for mechanical damage:
   - Broken springs between joints
   - Detached screw nuts/connectors
   - Debris in screw mechanisms
2. Check for finger interference (thumb/index finger)

---

## Software

### 1. Q: How do I program ROHand?

**A:** Use ModBus-RTU protocol to write to position registers:

- Registers: `ROH_FINGER_POS_TARGET0` to `ROH_FINGER_POS_TARGET5`
- Values: 0 (fully open) to 65535 (fully closed)  
Thumb rotation: 0 (0° side) to 65535 (90° palm) at  

Reference:  
[roh_registers_v1.h](../protocol/roh_registers_v1.h) / [roh_registers_v1.py](../protocol/roh_registers_v1.py)  
Sample code: [Appendix 1](#appendix-1-basic-control)

---

### 2. Q: How do I read/control finger angles?

**A:**
**1. Read Finger Angles:**

- Register Addresses: `ROH_FINGER_ANGLE0` to `ROH_FINGER_ANGLE5`  
- If read value (signed int) ≥ 32768:  `Actual Angle = (Read Value - 65535) / 100`  
- If read value (signed int) < 32768:  `Actual Angle = (Read Value) / 100`  

**2. Set Finger Angles:**

- Write Registers: `ROH_FINGER_ANGLE_TARGET0` to `ROH_FINGER_ANGLE_TARGET5`  
- If input value (signed int) ≥ 32768: `Set Angle = (Desired Angle - 65535) × 100`  
- If input value (signed int) < 32768: `Set Angle = (Desired Angle) × 100`  

**Important**  

- If (Angle Lower Limit) ≤ (Set Angle) ≤ (Angle Upper Limit): `Read Angle = Set Angle`  
- If (Set Angle) ≥ (Angle Upper Limit): `Read Angle = Angle Upper Limit`  
- If (Set Angle) ≤ (Angle Lower Limit): `Read Angle = Angle Lower Limit`

**Example:**  
Set index finger to 101.01° → Write 10101 to `ROH_FINGER_ANGLE_TARGET1`  
Sample code: [Appendix 1](#appendix-1-basic-control)

**Note:** Angles measured between first joint and palm plane.  
Protocol: [OHandModBusRTUProtocol_CN.md](../protocol/OHandModBusRTUProtocol_CN.md)

---

### 3. Q: How do I determine finger angle ranges?

**A:** Actual ranges vary per unit. To measure:  

1. Write 0 to `ROH_FINGER_POS_TARGETx` → Read `ROH_FINGER_ANGLEX` (max  angle)
2. Write 65535 → Read register (min angle)  

Sample code: [Appendix 2](#appendix-2-measure-angle-ranges)

---

### 4. Q: How can I detect object grasping?

**A:** Use `ROH_FINGER_STATUSx`to detect contact:

- Detect grasping via `ROH_FINGER_STATUS0`-`ROH_FINGER_STATUS5`
- Status code 5 (`STATUS_STUCK`) indicates contact

---

### 5. Q: Why do fingers shake during movement?

**A:** Caused by suboptimal PID tuning. Adjust parameters using OHandSetting.exe.

---

### 6. Q: What are ROHand's maximum baud rate and command frequency?

**A:**

- **RS485:** 112500 bps
- At 115200 bps: 60 Hz command rate
- With bundling: 90-100 Hz
- **CAN:** 1 Mbps baud rate

---

### 7. Q: What control modes are available?

**A:** Supports:

- **Position Control:** Direct motor positioning
- **Angle Control:** Converts angles to positions

---

### 8. Q: What algorithm controls the motors?

**A:** PID-based position control with:

- Real-time speed/current monitoring
- Stall protection (>500mA → pulsed retries)
- Heat reduction logic: [Appendix 3](#appendix-3-reduce-heat-from-motor-stalls)

---

### 9. Q: How do I install USB-to-485 drivers on Ubuntu?

**A:**

1. Install CH340 driver: [Download](https://www.wch.cn/downloads/CH341SER_EXE.html)
2. Device appears as `/dev/ttyUSB0`
3. Verify: `ls /dev/ttyUSB*`
4. If missing, check for `brltty` conflicts

---

### 10. Q: How do I prevent shaking and overheating?

**A:**

- Avoid frequent target updates
- Implement directional control:[Appendix 4](#appendix-4-prevent-shakingoverheating)
- Send commands only when direction changes

---

## Appendices

### Appendix 1: Basic Control

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

    # Make a fist
    resp = client.write_registers(ROH_FINGER_POS_TARGET1, [65535, 65535, 65535, 65535, 65535], NODE_ID)
    time.sleep(2)
    resp = client.write_registers(ROH_FINGER_POS_TARGET0, 65535, NODE_ID)
    time.sleep(2)

    # Open
    resp = client.write_registers(ROH_FINGER_POS_TARGET0, 0, NODE_ID)
    time.sleep(2)
    resp = client.write_registers(ROH_FINGER_POS_TARGET1, [0, 0, 0, 0, 0], NODE_ID)
    time.sleep(2)

    # Write finger angle, the value written is actual value * 100
    real_angle = 15.05
    target_angle = round(real_angle * 100)

    if (target_angle < 0) :
        target_angle += 65536

    resp = client.write_registers(ROH_FINGER_ANGLE_TARGET0, [target_angle], NODE_ID)
    time.sleep(2)
    
    # Read the current finger angle, the actual value is output value / 100
    resp = client.read_holding_registers(ROH_FINGER_ANGLE0, 1, NODE_ID)
    current_angle = resp.registers

    if (current_angle > 32767) :
        current_angle -= 65536

    current_angle = current_angle  / 100.0

    print("Current finger angle：", current_angle)
```

### Appendix 2: Measure Angle Ranges

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

    # Make a fist
    resp = client.write_registers(ROH_FINGER_POS_TARGET1, [65535, 65535, 65535, 65535, 65535], NODE_ID)
    time.sleep(2)
    resp = client.write_registers(ROH_FINGER_POS_TARGET0, 65535, NODE_ID)
    time.sleep(2)

    # Open
    resp = client.write_registers(ROH_FINGER_POS_TARGET0, 0, NODE_ID)
    time.sleep(2)
    resp = client.write_registers(ROH_FINGER_POS_TARGET1, [0, 0, 0, 0, 0], NODE_ID)
    time.sleep(2)

    # Write finger angle, the value written is actual value * 100
    real_angle = 15.05
    target_angle = round(real_angle * 100)

    if (target_angle < 0) :
        target_angle += 65536

    resp = client.write_registers(ROH_FINGER_ANGLE_TARGET0, [target_angle], NODE_ID)
    time.sleep(2)
    
    # Read the current finger angle, the actual value is output value / 100
    resp = client.read_holding_registers(ROH_FINGER_ANGLE0, 1, NODE_ID)
    current_angle = resp.registers

    if (current_angle > 32767) :
        current_angle -= 65536

    current_angle = current_angle  / 100.0

    print("Current finger angle：", current_angle)
```

### Appendix 3: Reduce Heat from Motor Stalls

```python
# Sample code to get glove data and controls ROHand via ModBus-RTU protocol

import asyncio
import os
import signal
import sys
import time

from pymodbus import FramerType
from pymodbus.client import ModbusSerialClient

from roh_registers_v1 import *

# ROHand configuration
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

        # Open all fingers
        self.write_registers(client, ROH_FINGER_POS_TARGET0, [0, 0, 0, 0, 0], NODE_ID)
        time.sleep(1.5)

        # Rotate thumb root to opposite
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
                    # If finger is stuck, set target position to current position
                    resp = client.read_holding_registers(ROH_FINGER_POS0 + i, 1, NODE_ID)
                    pos_copy[i] = resp.registers
                else:
                    pos_copy[i] = pos[i]

            self.write_registers(client, ROH_FINGER_POS_TARGET0, pos, NODE_ID)


if __name__ == "__main__":
    app = Application()
    asyncio.run(app.main())
```

### Appendix 4: Prevent Shaking/Overheating

```python


TOLERANCE = round(65536 / 32) # Determine the threshold for target position changes. Integer in position control mode, float in angle control mode.
SPEED_CONTROL_THRESHOLD = 8192 # When the position change is below this value, linearly adjust the finger movement speed.

prev_dir = [0 for _ in range(NUM_FINGERS)]
prev_finger_data = [0 for _ in range(NUM_FINGERS)]

while True:
    finger_data = get_latest_data() # Obtain target position/angle

    dir = [0 for _ in range(NUM_FINGERS)]
    pos = [0 for _ in range(NUM_FINGERS)]
    target_changed = False

    for i in range(NUM_FINGERS):
        if finger_data[i] > prev_finger_data[i] + TOLERANCE:
            prev_finger_data[i] = finger_data[i]
            dir[i] = 1
        elif finger_data[i] < prev_finger_data[i] - TOLERANCE:
            prev_finger_data[i] = finger_data[i]
            dir[i] = -1

        # Only send the target position/angle when the direction changes.
        if dir[i] != prev_dir[i]:
            prev_dir[i] = dir[i]
            target_changed = True

        if dir[i] == -1:
            pos = 0
        elif dir[i] == 0:
            pos = finger_data[i]
        else:
            pos = 65535

        if target_changed:
            # Obtain the current position.
            curr_pos = [0 for _ in range(NUM_FINGERS)]
            resp = client.read_holding_registers(ROH_FINGER_POS0, NUM_FINGERS, NODE_ID)
            curr_pos = resp.registers

            speed = [0 for _ in range(NUM_FINGERS)]

            for i in range(NUM_FINGERS):
                temp = interpolate(abs(curr_pos[i] - finger_data[i]), 0, SPEED_CONTROL_THRESHOLD, 0, 65535)
                speed[i] = clamp(round(temp), 0, 65535)

            # Set speed
            resp = client.write_register(ROH_FINGER_SPEED0, speed, NODE_ID)
            print(f"client.write_register({ROH_FINGER_SPEED0}, {speed}, {NODE_ID}) returned", resp)

            # Control ROHand
            resp = client.write_register(ROH_FINGER_POS_TARGET0, pos, NODE_ID)
            print(f"client.write_register({ROH_FINGER_POS_TARGET0}, {pos}, {NODE_ID}) returned", resp)
```
