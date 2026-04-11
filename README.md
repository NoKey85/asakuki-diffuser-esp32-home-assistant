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

* ESP32 (ESP32 Dev Board)
* DC-DC Buck Converter (24V → 5V, MP1584EN)
* Asakuki 500ml Diffuser
* Basic soldering tools and wiring
* 22AWG needed for DC-DC --> Buck Converter
* 28AWG female jumper wire needed 

---

## ⚙️ How It Works

The ESP32 is wired directly to the diffuser’s button PCB.

Each GPIO pin simulates a button press using **open-drain output**, safely pulling the signal to ground—just like a real button press.

Power is tapped from the diffuser’s 24V input and stepped down to 5V using a buck converter to power the ESP32 internally.

---

## 🧠 Important Behavior Notes

This diffuser does **not** expose independent controls.

* Buttons cycle through modes (state machine)
* Some buttons affect multiple functions
* Automations should use **button sequences**, not assumed states

---

## 📁 ESPHome Configuration

See: `esphome/diffuser.yaml`

---

## 📸 Wiring & Teardown

See: `docs/` folder

---

## ⚠️ Lessons Learned / Gotchas

* Buck converter must be tuned **under load**
* Micro USB wiring caused voltage drop → direct 5V pin works best
* Poor solder joints can cause voltage collapse under load
* Miswiring can cause cross-triggering (e.g., mist triggering light)

---

## 🛠️ Status

* ✅ Complete Build-guide (Docs/Build-guide.md)
* ✅ 📸 Build Photos

See the `Docs/Diffuser Pics` folder for full reference images.

---

## ⚠️ Disclaimer

This mod requires soldering and working with power circuits.
Attempt at your own risk.
Solder iron tips are HOT, dont be like me and touch them when powered on.
