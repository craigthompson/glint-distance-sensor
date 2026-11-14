# Glint Distance Sensor

A general-purpose Wi-Fi distance sensor for Home Assistant, built on ESPHome: an ESP32-C3 Super Mini and a VL53L0X time-of-flight sensor.

**Install, set up and update:** https://craigthompson.github.io/glint-distance-sensor/

## Features

- Distance in mm, averaged over 1–10 samples per measurement (default 5)
- Optional percentage mapped between a user-set 0% and 100% distance
- Adjustable measurement interval (5 s – 24 h, default 30 min) and a **Measure Now** button
- Status LED breathes while idle and flashes while measuring; dimmable or off from Home Assistant
- No Wi-Fi credentials or keys in the firmware. Onboarding over Improv BLE (with BOOT-button authorization), a branded captive portal, or Improv Serial
- Firmware updates delivered to Home Assistant from GitHub releases
- Measurement-quality, device-health and Wi-Fi diagnostics (see below)

## Home Assistant entities

**Default** is how the entity starts out in Home Assistant. "Disabled" entities exist, but are hidden until you turn them on: open the device page, click **+N disabled entities**, select the entity, then ⚙️ → **Enabled**.

Home Assistant may convert units to your preferred system, e.g. inches instead of mm or °F instead of °C.

### Measurements

Published after every measurement: every 30 min by default, about 10 s after Home Assistant connects, and when you press **Measure Now**.

| Entity | Unit | Default | What it is |
|---|---|---|---|
| Distance | mm | Enabled | Distance to the target, averaged over the valid samples of one measurement. **Unknown** when nothing is in range (about 2 m maximum; less for dark or angled targets) or the sensor has a problem. |
| Percentage | % | Enabled | Distance mapped between your **0% Distance** and **100% Distance** settings, limited to 0–100. Unknown until both settings are set to different values. Works either way round, e.g. 0% = empty tank (far), 100% = full (near). |

### Measurement quality (diagnostic)

Also published after every measurement.

| Entity | Unit | Default | What it is |
|---|---|---|---|
| Sensor Problem | on/off | Enabled | **On** when the sensor stops responding: setup failed, no reading came back, or a reading never finished. The device then resets the sensor and restarts, up to 3 times. **Off** after a good measurement, or when nothing is in range. Use it for a "sensor needs attention" automation. |
| Valid Samples | count | Enabled | How many valid samples the last measurement averaged. Normally equals **Samples per Measurement**. The device keeps trying, up to twice as many attempts, so lower numbers mean the target is at the edge of range. |
| Reading Spread | mm | Enabled | Largest minus smallest valid sample in the last measurement. A few mm is normal; a large spread means a noisy or uneven surface. Unknown with fewer than 2 samples. |
| Last Measurement | date/time | Enabled | When the last measurement ran. The time comes from Home Assistant, so it's blank until the device has synced its clock. |
| Ambient IR | kcps | Enabled | Infrared light around the sensor, in thousands of photon counts per second. The VL53L0X only sees infrared at 940 nm, the wavelength of its own laser, so this isn't visible light and can't be converted to lux or lumens. Sunlight and incandescent bulbs read high; LED and fluorescent lighting reads near zero. High values add noise and shorten the range. |
| Ambient IR Level | Low / Moderate / High | Enabled | **Ambient IR** in plain language: Low below 200 kcps, Moderate up to 1000 kcps, High above. These thresholds are provisional; set them with `ambient_ir_moderate_kcps` and `ambient_ir_high_kcps` in `glint-distance-sensor.yaml`. |
| Signal Strength | kcps | Disabled | How much of the sensor's own laser light comes back from the target. Higher is better. It drops with distance and with dark, shiny or angled targets. Compare it with Ambient IR when readings are unreliable. |

### Device health (diagnostic)

| Entity | Unit | Updates | Default | What it is |
|---|---|---|---|---|
| Wi-Fi Signal | dBm | 5 min | Enabled | Wi-Fi signal strength. Above −67 dBm is good; below −80 dBm can cause dropouts. |
| Uptime | s | 60 s | Enabled | Time since the device last started. |
| Chip Temperature | °C | 5 min | Enabled | Temperature of the ESP32-C3 chip itself, not the room. It normally reads well above room temperature; watch it for changes, not the absolute value. |
| Last Restart Reason | text | At boot | Enabled | Why the device last started, e.g. power-on, software restart (updates and the Restart button), crash or watchdog. Useful when troubleshooting. |
| Firmware Version | text | At boot | Enabled | ESPHome version and config hash the firmware was built with. The Glint release is shown on the device page and the **Firmware** update entity. |
| Free Memory | bytes | 5 min | Disabled | Free heap memory. A steady drop across days points to a memory leak. |
| Loop Time | ms | 5 min | Disabled | Longest main-loop pass in the last interval. Spikes mean something is blocking the device. |
| IP Address | text | On change | Disabled | The device's IP address. |
| Wi-Fi Network | text | On change | Disabled | Name (SSID) of the network it's connected to. |
| Access Point | text | On change | Disabled | MAC address (BSSID) of the access point it's connected to. Useful with mesh or multi-access-point Wi-Fi. |

### Controls and settings

| Entity | Type | Default | What it does |
|---|---|---|---|
| Measure Now | Button | Enabled | Takes a measurement immediately. |
| 0% Distance / 100% Distance | Number (mm) | Enabled | The ends of the **Percentage** range. Default 0, meaning unset. |
| Measurement Interval | Number (s) | Enabled | Time between automatic measurements: 5 s to 24 h, default 1800 s (30 min). |
| Samples per Measurement | Number | Enabled | Valid samples averaged per measurement: 1–10, default 5. |
| Status LED | Light | Enabled | Breathes while idle and flashes while measuring. Turn it off or dim it here; picking the **None** effect makes it light steadily. |
| Restart | Button | Enabled | Restarts the device. |
| Factory Reset | Button | Disabled | Forgets Wi-Fi and all settings, same as holding BOOT for 10 s. |
| Firmware | Update | Enabled | Shows and installs new releases. Checks every 6 hours and at boot. *Factory firmware only.* |
| Check for Firmware Update | Button | Enabled | Checks for a new release right away. *Factory firmware only.* |

Devices taken over in the ESPHome dashboard (**Take Control**) build their own firmware, so they don't have the two *factory firmware only* entities.

## Hardware

| VL53L0X | ESP32-C3 Super Mini |
|---|---|
| VIN | 3.3V |
| GND | GND |
| SCL | IO6 |
| SDA | IO7 |
| XSHUT | IO5 (optional, see below) |

Onboard LED: IO8 (active low). BOOT button: IO9.

XSHUT lets the firmware fully reset the sensor. Until it's wired, `enable_pin: GPIO5` stays commented out in `glint-distance-sensor.yaml`; don't connect IO5 to the sensor's GPIO1 pin while it's enabled.

## Repository layout

| Path | Purpose |
|---|---|
| `glint-distance-sensor.yaml` | Core config. This is what a user's ESPHome dashboard imports on **Take Control**. |
| `glint-distance-sensor.factory.yaml` | Factory firmware: core + project info, `dashboard_import`, Improv BLE/Serial, and HTTP updates. Built by CI. |
| `components/captive_portal/` | ESPHome's `captive_portal` component with a custom setup page. |
| `tools/build_captive_portal.py` | Regenerates `captive_index.h` from `portal.html`. |
| `static/` | GitHub Pages site with the web installer. |

### Editing the setup page

Edit `components/captive_portal/portal.html`, then run:

```sh
python3 tools/build_captive_portal.py
```

The other files in `components/captive_portal/` are unmodified copies from ESPHome 2026.10.2. When you bump `esphome-version` in `.github/workflows/build.yml`, re-copy them from that ESPHome version.

## Releasing

Release notes appear in Home Assistant's update dialog, and they're built from merged pull requests. So make every change through a PR:

- **Title:** written for users, e.g. "Status LED resumes breathing when turned back on".
- **Label:** `enhancement` (New Features), `bug` (Bug Fixes), `breaking-change` (Breaking Changes), `documentation` or `maintenance` (collapsed sections), or `skip-changelog` to leave it out.

Commits pushed straight to `main` don't appear in the notes.

1. Merge PRs to `main`. **Release Drafter** creates or updates a draft release with a CalVer version, and **Build** compiles the factory firmware and attaches it.
2. When the build finishes, run **Actions → Release → Run workflow**. That publishes the release and deploys the Pages site, and the new version shows up as an update in Home Assistant.

## License

GPL-3.0. `components/captive_portal/` is derived from [ESPHome](https://github.com/esphome/esphome): its C++ code is GPLv3 and its Python code is MIT-licensed by the ESPHome authors.
