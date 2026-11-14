# Glint Distance Sensor

A Wi-Fi time-of-flight distance sensor for [Home Assistant](https://www.home-assistant.io/), running [ESPHome](https://esphome.io/).
Use it for salt and water tank levels, bins, parking, or anything else where knowing a distance (up to about 2 m) is useful.

It reports:

- **Distance** in millimetres, averaged over several samples
- **Percentage** between a 0% and a 100% distance you choose, e.g. "salt tank empty" and "salt tank full"

Measurements run every 30 minutes by default; you can change the interval, the number of samples and the status LED from Home Assistant.

## Install firmware

Plug the sensor into this computer with a USB **data** cable, then click the button. Works in Chrome or Edge on a computer.

<esp-web-install-button manifest="firmware/glint-distance-sensor.manifest.json"></esp-web-install-button>

After installing, the installer offers to connect the sensor to your Wi-Fi.

<script type="module" src="esp-web-tools/install-button.js"></script>

## Connect to Wi-Fi

Use whichever is easiest:

1. **Home Assistant (Bluetooth).** If your Home Assistant has Bluetooth, it shows *Discovered: Improv via BLE*. Click **Add**, press the sensor's **BOOT** button when asked, then choose your Wi-Fi network.
2. **Setup hotspot.** On your phone, join the Wi-Fi network **Glint Distance Sensor Setup**. If a setup page doesn't open by itself, browse to `http://192.168.4.1`. Pick your network and enter its password.
3. **USB.** Use the installer above and choose *Connect to Wi-Fi*.

The sensor only supports 2.4 GHz Wi-Fi.

## Add to Home Assistant

Once the sensor is on your Wi-Fi, Home Assistant discovers it: go to **Settings → Devices & services**, find **ESPHome**, and click **Add**.

Then set **0% Distance** and **100% Distance** on the device page if you want a percentage.

## Updates

New firmware appears in Home Assistant as an update for the sensor's **Firmware** entity. Install it like any other update.

## Buttons

- **BOOT, short press:** authorizes Bluetooth Wi-Fi setup for 2 minutes.
- **BOOT, hold 10 seconds:** factory reset (forgets Wi-Fi and all settings).
- **RESET:** restarts the sensor.

## Make it your own

Use the ESPHome dashboard? Discovered sensors show **Take Control**, which adds an editable configuration to your dashboard. After that, you build and update the firmware yourself, and it no longer receives updates from this project.

[Source code on GitHub](https://github.com/craigthompson/glint-distance-sensor)
