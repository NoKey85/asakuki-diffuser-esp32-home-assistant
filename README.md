# Smart Diffuser (ESP32 + Home Assistant)

Convert an Asakuki diffuser into a fully local, Home Assistant-controlled smart device using ESP32 and ESPHome.

---

## 🚀 Features

* 100% local control (no cloud required)
* Native Home Assistant integration via ESPHome
* Simulates physical button presses (no IR needed)
* Fully internal mod
* Original buttons still work

---

## 🧰 Hardware Used

This project has two parts:
- Physical diffuser modification (hardware build)
- Home Assistant automation (control + monitoring)

---

## 🔧 Build Components (Hardware)

These are required to modify the diffuser and add ESP32 control.

### Core Components
- **Asakuki Essential Oil Diffuser**
  - Any model with physical push buttons (power/timer + light)

- **ESP32 Development Board**
  - 30-pin ESP32 (approx. 51mm x 28mm)
  - Used for ESPHome integration and button control

### Wiring & Electronics
- **Wire (16–22 AWG)**
  - Used to connect ESP32 GPIO pins to diffuser button contacts
  - Stranded wire is easier to work with than solid core

- **Soldering Kit**
  - Soldering iron
  - Solder (rosin core recommended)
  - Flux (recommended for clean joints)

### Optional but Recommended
- **Heat shrink tubing or electrical tape**
  - Insulates solder joints

- **Hot glue or mounting tape**
  - Secures ESP32 inside diffuser housing

- **USB Power Cable**
  - Used to power the ESP32

---

## 🧠 Automation Components (Home Assistant)

These are required for reliable control and automation.

### Core Requirements
- **Home Assistant instance**
  - Raspberry Pi, VM, or server

- **ESPHome integration**
  - Used to control ESP32 and expose button entities

---

### Critical Component (Required for Automation Logic)

- **Smart Plug with Power Monitoring**
  - Zigbee, Z-Wave, or WiFi
  - MUST support real-time wattage reporting

Example:
- Minoston Z-Wave plug
- Third Reality Zigbee plug

This is REQUIRED because:
- The diffuser does not report its state
- Power usage is used to determine ON/OFF status

---

### Optional (But Recommended)
- **Zigbee or Z-Wave Coordinator**
  - If using Zigbee/Z-Wave smart plugs

- **Home Assistant Notifications**
  - Mobile app or notification service (e.g., `notify.matt_notifications`)

---

## ⚙️ Why These Components Are Needed

### ESP32
- Simulates physical button presses
- Integrates diffuser into Home Assistant

### Smart Plug with Power Monitoring
- Detects whether diffuser is actually running
- Enables:
  - runtime tracking
  - automation decisions
  - low-water detection

### Home Assistant + ESPHome
- Provides:
  - automation logic
  - scheduling
  - state tracking
  - notifications

---

## 🧩 System Overview

The system works by combining:

- ESP32 → sends button presses  
- Smart plug → reports power usage  
- Home Assistant → makes decisions  

This allows reliable automation even though the diffuser itself does not expose a true state. 

---

## ⚙️ How It Works

The ESP32 is wired directly to the diffuser’s button PCB.

Each GPIO pin simulates a button press using **open-drain output**, safely pulling the signal to ground—just like a real button press.

Power is tapped from the diffuser’s 24V input and stepped down to 5V using a buck converter to power the ESP32 internally.

---

## 📁 Project Files

- [Build Guide](./docs/build-guide.md)
- [ESPHome Config](./esphome/diffuser.yaml)
- [Build Photos](./docs/Diffuser%20Pics/)
- [Home Assistant Automation Guide](docs/home-assistant-automation.md)


## 🚀 Start Here

1. Follow the [Build Guide](docs/build-guide.md)
2. Flash ESPHome using [ESPHome Config](esphome/diffuser.yaml)
3. Set up Home Assistant automation using:
   - [Automation Guide](docs/home-assistant-automation.md)
   - [Example YAML](docs/home-assistant-automation-example.yaml)
  


## 🧠 Important Behavior Notes

This diffuser does **not** expose independent controls.

* Buttons cycle through modes (state machine)
* Some buttons affect multiple functions
* Automations should use **button sequences**, not assumed states
* Because the diffuser buttons cycle through modes instead of exposing direct state, the Home Assistant automation uses smart-plug power monitoring, helper entities, reset scripts, and button-press sequences to reliably control runtime.

---

## 📁 ESPHome Configuration

See: `esphome/diffuser.yaml`

---

## 📸 Wiring & Teardown

See the [Build Guide](./docs/build-guide.md) and [Build Photos](./docs/Diffuser%20Pics/) for teardown, wiring, and assembly reference.

---

## ⚠️ Lessons Learned / Gotchas

* Buck converter must be tuned **under load**
* Micro USB wiring caused voltage drop → direct 5V pin works best
* Poor solder joints can cause voltage collapse under load
* Miswiring can cause cross-triggering (e.g., mist triggering light)

---

## 🛠️ Status

* ✅ Complete Build-guide (docs/build-guide.md)
* ✅ 📸 Build Photos

See the `docs/Diffuser Pics` folder for full reference images.

---

## ⚠️ Disclaimer

This mod requires soldering and working with power circuits.
Attempt at your own risk.
Solder iron tips are HOT, dont be like me and touch them when powered on.
