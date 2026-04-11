# Home Assistant Automation Guide

This project does not expose true discrete controls for mist runtime. The diffuser behaves like a state machine:

- The power/timer button cycles through:
  - 1 press = On (no timer)
  - 2 presses = 60 minutes
  - 3 presses = 120 minutes
  - 4 presses = 180 minutes
  - 5 presses = Off
- The original physical buttons still work, so Home Assistant cannot safely assume the diffuser is in a known state at all times.

To make the automation reliable, I paired each diffuser with a smart plug that reports power consumption. Home Assistant uses plug wattage to determine whether the diffuser is actually running, stores the runtime start time in helpers, and power-cycles the plug when needed so the diffuser always starts from a known baseline before sending button presses.

---

## My Setup

### Bedroom diffuser
- Smart plug: `switch.masterbedroom_diffuser_zig_plug`
- Power sensor: `sensor.masterbedroom_diffuser_zig_plug_power`
- Timer button: `button.bedroom_diffuser_diffuser_power_timer`

### Living room diffuser
- Smart plug: `switch.livingroom_zig_plug`
- Power sensor: `sensor.livingroom_zig_plug_power`
- Timer button: `button.livingroom_diffuser_esp_diffuser_power_timer`

---

## Power Detection Logic

For both of my diffusers:

- Below 4W = diffuser is off
- Above 4W = diffuser is on

Home Assistant tracks this with helper entities and automations:

- `input_boolean.bedroom_diffuser_running_flag`
- `input_datetime.bedroom_diffuser_runtime_start`
- `sensor.bedroom_diffuser_runtime_minutes`

- `input_boolean.livingroom_diffuser_running_flag`
- `input_datetime.livingroom_diffuser_runtime_start`
- `sensor.livingroom_diffuser_runtime_minutes`

The runtime sensor is based on the helper timestamp, so Home Assistant can make timer decisions based on how long the diffuser has already been running.

---

## Why Reset Scripts Are Needed

Because the diffuser buttons cycle through states, Home Assistant cannot simply "set 120 minutes" directly.

Instead, I use a reset script that:

1. Turns the smart plug off
2. Waits 2 seconds
3. Turns the smart plug back on
4. Waits 6 seconds

That guarantees the diffuser is back in a known off state before Home Assistant starts pressing buttons.

---

## Timer Mapping From a Known Off State

From a clean reset:

- 2 presses = 60 minutes
- 3 presses = 120 minutes
- 4 presses = 180 minutes

---

## Runtime Logic Used

### For a 120-minute request
- If off: start 120
- If on and elapsed is 0–59 min: reset and start 120
- If on and elapsed is 60–120 min: reset and start 60
- If on and elapsed is over 120 min: reset and leave off

### For a 180-minute request
- If off: start 180
- If on and elapsed is 0–29 min: reset and start 180
- If on and elapsed is 30–99 min: reset and start 120
- If on and elapsed is 100–180 min: reset and start 60
- If on and elapsed is over 180 min: reset and leave off

---

## Schedule I Currently Use

### Bedroom
- 6:00 AM = 120 minutes
- 3:30 PM = 120 minutes
- 9:30 PM = 120 minutes

### Living room
- 6:00 AM = 120 minutes
- 3:30 PM = 120 minutes
- 7:30 PM = 120 minutes

---

## Low Water Notification

When the automation starts the diffuser, Home Assistant also watches for an early shutoff.

If the diffuser turns off again within about 3 minutes of an automated start, a notification is sent to:

`notify.matt_notifications`

This is used as a practical “check the water level” alert.

---

## Why This Approach Works

This setup is reliable because it does not try to guess the diffuser state from previous button presses alone.

Instead, it uses:

- Real power consumption from the smart plug
- Helper entities to track runtime
- Reset scripts to force a known starting point
- Button-press sequences only after reset

That makes it much more dependable than trying to automate the diffuser as if it had direct on/off/timer entities.
