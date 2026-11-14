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
