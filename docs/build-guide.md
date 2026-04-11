# 🛠️ Build Guide – Asakuki Diffuser ESP32 Mod

This guide walks through converting an Asakuki diffuser into a fully local Home Assistant device using an ESP32.

---

## 🥇 Step 1 – Open the Diffuser

* Flip the diffuser upside down
* Remove screws from the base
* Carefully separate the bottom housing

📸 Refer to ![Bottom screw holes](./Diffuser%20Pics/1_Bottom%20screw%20holes.JPG)

---

## 🥈 Step 2 – Identify Internal Components

Inside the diffuser you will find:

* Power barrel jack (24V input)
* Main control board
* Button PCB (3 buttons)
* Ribbon cable connecting boards

📸 Refer to ![Internal Layout](./Diffuser%20Pics/2_Internal%20layout.jpg) 

---

## 🥉 Step 3 – Tap Power (24V Input)

Locate the **barrel jack** where power enters the device.

### You will:

* Solder wires directly to:

  * **Positive (red)**
  * **Negative (black / ground)**

📸 Refer to ![Barrel Jack Solder](./Diffuser%20Pics/3_Barrel%20Jack%20Solder.jpg)

⚠️ Tips:

* Use **stranded wire**
* Keep joints clean and solid
* Avoid bridging contacts

---

## 🥇 Step 4 – Install Buck Converter (24V → 5V)

Wire the buck converter:

### Input side:

* IN+ → 24V positive
* IN- → ground

### Output side:

* OUT+ → ESP32 5V / VIN
* OUT- → ESP32 GND

📸 Refer to images ![Buck Solering Input](./Diffuser%20Pics/4_Buck%20Soldering_Input.jpg)
![Buck Soldering Output](./Diffuser%20Pics/4_Buck%20Soldering_Output.jpg)

---

### 🔧 IMPORTANT: Voltage Adjustment

* Adjust buck converter to **5V**
* **Must test under load** (ESP32 connected)

⚠️ If voltage drops under load:

* Check solder joints
* Use thicker wire on input
* Ensure solid connections

  
📸 Refer to image ![Buck Soldering Output](./Diffuser%20Pics/6_Board%20Testing%20Underload.jpg)

---

## 🥈 Step 5 – Power ESP32

Do **NOT rely on micro USB wiring**

Instead:

* Connect directly:

  * Buck OUT+ → ESP32 **5V/VIN**
  * Buck OUT- → ESP32 **GND**

---

## 🥉 Step 6 – Identify Button PCB

There are 3 buttons:

* **S5 → Power / Timer / Off**
* **S4 → Mist**
* **S3 → Light**

Each button has:

* Signal side (top)
* Ground side (bottom)

📸 Refer to image ![PCB Board Identify and soldering](./Diffuser%20Pics/7_PCB%20Board%20Identify%20and%20soldering.jpg)

---

## 🥇 Step 7 – Wire ESP32 to Buttons

### Wiring:

* GPIO23 → Power button (S5 signal)

* GPIO22 → Mist button (S4 signal)

* GPIO21 → Light button (S3 signal)

* All button grounds → ESP32 GND

📸 Refer to image ![PCB Board Identify and soldering](./Diffuser%20Pics/7_PCB%20Board%20Identify%20and%20soldering.jpg)

---

### 🧠 Important

* Buttons share **common ground**
* ESP32 uses **open-drain output**
* This simulates real button presses safely

---

## 🥈 Step 8 – ESPHome Configuration

Use GPIO pins configured as:

* `open_drain: true`
* `inverted: true`
* short pulse (100ms)

See: `esphome/diffuser.yaml`

---

## 🥉 Step 9 – Test Before Assembly

Before closing:

* Power on ESP32
* Confirm WiFi connection
* Test each button from Home Assistant

### Expected behavior:

* Power button cycles timer
* Mist cycles modes
* Light cycles colors

---

## 🥇 Step 10 – Final Assembly

* Secure wires (hot glue recommended)
* Ensure no exposed contacts
* Keep buck converter isolated
* Reassemble housing

📸 Final assembled image ![Final internal Layout](./Diffuser%20Pics/8_Final%20Layout.jpg)
8_Final Layout (No hot glue, but was used later to secure everything).

---

# ⚠️ Troubleshooting

### ESP32 won’t power on

* Check buck output under load
* Avoid micro USB wiring
* Verify 5V at ESP32 pins

---

### Voltage drops under load

* Poor solder joints
* Wire too thin on input side
* Buck converter not adjusted under load

---

### Buttons trigger wrong function

* Solder bridge between pads
* Incorrect GPIO mapping
* Shared ground not correct

---

### Random button presses

* Missing `open_drain`
* Floating GPIO
* Bad ground connection

---

# 🧠 Notes

* This diffuser uses a **state machine**
* Buttons cycle modes instead of direct control
* Automations should use sequences, not fixed states

---

# 🏁 Result

* Fully local smart diffuser
* No cloud required
* Integrated into Home Assistant
* Original buttons still functional
