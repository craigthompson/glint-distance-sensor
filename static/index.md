---
layout: default
---

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

After installing, the installer offers to connect the sensor to your Wi-Fi. You can skip that and use the [Quick start]({{ site.baseurl }}/setup/) instead.

<script type="module" src="esp-web-tools/install-button.js"></script>

## Next: set it up

After installing, follow the **[Quick start]({{ site.baseurl }}/setup/)** to connect the sensor to Wi-Fi, add it to Home Assistant and mount it. It also explains the status light, updates and factory reset.

## Make it your own

Use the ESPHome dashboard? Discovered sensors show **Take Control**, which adds an editable configuration to your dashboard. After that, you build and update the firmware yourself, and it no longer receives updates from this project.

