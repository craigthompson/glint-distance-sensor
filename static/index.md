# Glint Distance Sensor

A Wi-Fi time-of-flight distance sensor for [Home Assistant](https://www.home-assistant.io/), running [ESPHome](https://esphome.io/).
Use it for salt and water tank levels, bins, parking, or anything else where knowing a distance (up to about 2 m) is useful.

It reports:

- **Distance** in millimetres, averaged over several samples
- **Percentage** between a 0% and a 100% distance you choose, e.g. "salt tank empty" and "salt tank full"

Measurements run every 30 minutes by default; you can change the interval, the number of samples and the status LED from Home Assistant.

## Install firmware

Plug the sensor into this computer with a USB **data** cable, then click the button. Works in Chrome or Edge on a computer.

<div class="install"><esp-web-install-button manifest="firmware/glint-distance-sensor.manifest.json"><div slot="unsupported" class="note">This browser can't install firmware over USB. Open this page in Chrome or Edge on a computer.</div><div slot="not-allowed" class="note">Installing over USB needs a secure connection. Open this page using https://.</div></esp-web-install-button></div>

After installing, the installer offers to connect the sensor to your Wi-Fi.

<script type="module" src="esp-web-tools/install-button.js"></script>

## Connect to Wi-Fi

When the sensor's light blinks slowly, it's ready for Wi-Fi setup. Use whichever is easiest:

1. **Setup hotspot (recommended).** On your phone, join the Wi-Fi network **Glint Distance Sensor Setup**. If a setup page doesn't open by itself, browse to `http://192.168.4.1`. Tap your network in the list and enter its password.
2. **Home Assistant (Bluetooth).** If Home Assistant has Bluetooth near the sensor, it shows *Discovered: Improv via BLE*. Click **Add**, then type your Wi-Fi network's name and password exactly. If it doesn't appear, use the hotspot instead.
3. **USB.** Use the installer above and choose *Connect to Wi-Fi*, then pick your network from the list.

The sensor only supports 2.4 GHz Wi-Fi.

## Add to Home Assistant

Once the sensor is on your Wi-Fi, its light blinks twice and pauses, and Home Assistant discovers it: go to **Settings → Devices & services**, find **ESPHome**, and click **Add**.

When it's added, the light starts breathing slowly. Then set **0% Distance** and **100% Distance** on the device page if you want a percentage.

## Updates

New firmware appears in Home Assistant as an update for the sensor's **Firmware** entity. Install it like any other update. The sensor checks every 6 hours; press **Check for Firmware Update** on its device page to check right away.

## Status light

| Light | Meaning |
|---|---|
| Slow blink (1 s on, 1 s off) | Needs Wi-Fi setup. Set it up over Bluetooth, the setup hotspot or USB. |
| Fast blink | Connecting to Wi-Fi. If it doesn't stop, the network is out of range or its password changed. |
| 2 blinks, pause | On Wi-Fi but not connected to Home Assistant. Add it in Home Assistant, or check that Home Assistant is running. |
| 3 blinks, pause | Sensor problem: the distance sensor isn't responding. |
| Very fast flicker | Counting quick power cycles for a factory reset. |
| Solid on | Installing a firmware update. Don't unplug it. |
| Slow breathing | Working normally. It flashes briefly while measuring. |

If you turn the **Status LED** off or dim it in Home Assistant, it stays off except for setup, Wi-Fi and update patterns, which always show.

## Factory reset

To make the sensor forget its Wi-Fi and all settings, for example after moving to a new network:

- **Unplug and replug it 5 times in a row**, plugging it back in within 10 seconds each time. After the fifth time it restarts in setup mode.
- Or press **Factory Reset** on its device page in Home Assistant (enable the entity first).

Then set it up again as above.

## Make it your own

Use the ESPHome dashboard? Discovered sensors show **Take Control**, which adds an editable configuration to your dashboard. After that, you build and update the firmware yourself, and it no longer receives updates from this project.

