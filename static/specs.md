---
layout: default
permalink: /specs/
heading: Features & specifications
tagline: A precise, private, local distance sensor for Home Assistant
---

# Features & specifications

Glint Distance Sensor measures the distance to whatever it's pointed at, up to 2&nbsp;m (6.6&nbsp;ft), with a laser time-of-flight sensor, and reports it to [Home Assistant](https://www.home-assistant.io/) over Wi-Fi. Use it for salt and tank levels, bin fill levels, parking position, or anything else where a distance tells you something useful.

<div class="stats">
  <div><strong>2&nbsp;m <small>6.6&nbsp;ft</small></strong><span>maximum range</span></div>
  <div><strong>1&nbsp;mm <small>0.04&nbsp;in</small></strong><span>resolution</span></div>
  <div><strong>±3&nbsp;mm <small>±⅛ in</small></strong><span>repeatability at 1.65&nbsp;m (5.4&nbsp;ft)</span></div>
  <div><strong>1&nbsp;s</strong><span>fastest update interval</span></div>
  <div><strong>100%</strong><span>local, no cloud</span></div>
  <div><strong>2&nbsp;min</strong><span>setup, no app</span></div>
</div>

## Highlights

- **Laser precision.** A Class 1 eye-safe 940 nm laser times light's round trip to the target. The distance it reports doesn't depend on the target's colour, and unlike ultrasonic sensors it isn't thrown off by temperature or humidity or confused by echoes in narrow tanks.
- **Steady readings.** Each measurement averages up to 10 samples, and only valid ones. Repeated measurements of a fixed target stay within a few millimetres.
- **Made for Home Assistant.** Discovered automatically and added with one click. Distance, a fill-level percentage, and diagnostics appear as ready-made entities for dashboards and automations.
- **Private and local.** Talks only to your Home Assistant, over an encrypted connection. No account, no app, no cloud, and it keeps measuring when the internet is down.
- **Easy setup.** Connect it to Wi-Fi from your phone through its setup hotspot, from Home Assistant over Bluetooth, or over USB from a browser.
- **Looks after itself.** Firmware updates appear in Home Assistant. If the distance sensor ever stops responding, the device resets it and recovers by itself, and tells you if it can't.
- **Bonus Bluetooth proxy.** Optionally extends Home Assistant's Bluetooth range for sensors, locks and presence tracking.
- **Open source.** Built on [ESPHome](https://esphome.io/), with the full firmware [on GitHub](https://github.com/craigthompson/glint-distance-sensor). Take control in ESPHome and customise it any time.

## Measurement

| | |
|---|---|
| Technology | Laser time-of-flight (STMicroelectronics VL53L0X), single-photon avalanche diode receiver |
| Range | Up to 2&nbsp;m (6.6&nbsp;ft) indoors on light-coloured targets, about 80&nbsp;cm (31&nbsp;in) on dark ones (see [Range by target](#range-by-target)) |
| Resolution | 1&nbsp;mm (0.04&nbsp;in) |
| Repeatability | ±3&nbsp;mm (±0.12&nbsp;in, 1 standard deviation) at 1.65&nbsp;m (5.4&nbsp;ft), 5&#8209;sample measurements, measured over 353 measurements |
| Single-sample noise | About ±7&nbsp;mm (±0.28&nbsp;in, 1 standard deviation) at 1.65&nbsp;m (5.4&nbsp;ft); averaging removes most of it |
| Accuracy | Typically within 3% of the true distance (sensor datasheet) |
| Field of view | 25° cone: a spot about 22&nbsp;cm (9&nbsp;in) wide at 50&nbsp;cm (20&nbsp;in), 44&nbsp;cm (17&nbsp;in) at 1&nbsp;m (3.3&nbsp;ft), and 89&nbsp;cm (35&nbsp;in) at 2&nbsp;m (6.6&nbsp;ft) |
| Samples per measurement | 1–10, adjustable (default 5). Only valid samples are averaged, with up to twice as many attempts near the edge of range. |
| Measurement time | About 35&nbsp;ms per sample; 10 samples in about 0.6&nbsp;s |
| Update interval | 1&nbsp;s to 24&nbsp;h, adjustable (default 30&nbsp;min), plus on demand with **Measure Now** |
| Light source | 940 nm infrared laser (VCSEL), invisible, Class 1 eye-safe (IEC 60825-1:2014) |
{: .spec}

### Range by target

The maximum distance depends on how much light the target reflects and how much infrared light is around. Figures are from the sensor datasheet for a target that fills the field of view, with the long-range profile the firmware uses.

| Target | Indoors | Outdoors, overcast |
|---|---|---|
| White or light-coloured (88% reflectance) | 2&nbsp;m (6.6&nbsp;ft) typical, 1.2&nbsp;m (3.9&nbsp;ft) minimum | 80&nbsp;cm (31&nbsp;in) typical, 60&nbsp;cm (24&nbsp;in) minimum |
| Dark grey (17% reflectance) | 80&nbsp;cm (31&nbsp;in) typical, 70&nbsp;cm (28&nbsp;in) minimum | 50&nbsp;cm (20&nbsp;in) typical, 40&nbsp;cm (16&nbsp;in) minimum |

Sunlight on the sensor or the target shortens the range and adds noise; LED and fluorescent lighting don't. The **Ambient IR** reading in Home Assistant tells you how much infrared light the sensor sees.

## Home Assistant

| | |
|---|---|
| Integration | ESPHome, built into Home Assistant; no add-on needed |
| Discovery | Automatic (Zeroconf); Bluetooth setup shows up under Discovered |
| Connection | Local push over the ESPHome native API, encrypted with a key Home Assistant sets on adoption |
| Measurements | **Distance** (mm, converted to your units), **Percentage** between your own 0% and 100% distances |
| Measurement quality | Measurement Result, Valid Samples, Reading Spread, Signal Strength, Ambient IR and Ambient IR Level, Last Measurement, Sensor Problem |
| Device health | Wi-Fi Signal, Uptime, Chip Temperature, Last Restart Reason, Firmware Version, IP Address, Wi-Fi Network, Access Point, Free Memory, Loop Time |
| Controls | Measure Now, Scheduled Measurements on/off, Measurement Interval, Samples per Measurement, 0% and 100% Distance, Status LED, Bluetooth Proxy, Restart, Factory Reset |
| Updates | Firmware updates appear in Home Assistant and install with one click |
| Without Home Assistant | Keeps measuring on its schedule; Home Assistant gets the latest readings when it reconnects |
{: .spec}

The [project README](https://github.com/craigthompson/glint-distance-sensor#home-assistant-entities) describes every entity.

## Connectivity

| | |
|---|---|
| Wi-Fi | 2.4&nbsp;GHz 802.11 b/g/n. Power saving is off for fast, reliable replies (under 10&nbsp;ms on a typical network). |
| Bluetooth | Bluetooth 5 LE, for Wi-Fi setup and the optional Bluetooth proxy |
| Wi-Fi setup | Setup hotspot with a list of networks, Improv over Bluetooth from Home Assistant, or Improv over USB from a browser |
| Bluetooth proxy | Optional, off by default. Passive and active scanning; up to 3 simultaneous connections for Home Assistant. |
{: .spec}

## Hardware

| | |
|---|---|
| Controller | Espressif ESP32-C3, 32-bit RISC-V, 160&nbsp;MHz, 4&nbsp;MB flash |
| Power | 5&nbsp;V over USB-C, from any phone charger or USB port |
| Status light | One LED showing setup, Wi-Fi, Home Assistant, sensor and update status, with breathing and measuring patterns you can dim or turn off |
| Reset | Unplug and replug 5 times to factory reset, no tools needed |
| Operating temperature | −20 to 70&nbsp;°C (−4 to 158&nbsp;°F), the distance sensor's rating; indoor use |
{: .spec}

## Software

| | |
|---|---|
| Firmware | [ESPHome](https://esphome.io/), open source (GPL-3.0) |
| Updates | Published on GitHub and offered in Home Assistant; checked every 6 hours or on demand |
| Customising | **Take Control** in the ESPHome Device Builder imports the firmware's configuration, so you can change anything and build your own |
| Reinstalling | [Install firmware]({{ site.baseurl }}/firmware/) over USB from Chrome or Edge, no software to install |
| Privacy | No cloud, no account, no telemetry. The device only contacts GitHub to check for firmware updates. |
{: .spec}

## About these numbers

Range, accuracy, field of view, light source and temperature come from the STMicroelectronics VL53L0X datasheet (DocID029104 Rev 2), tested at 23&nbsp;°C (73&nbsp;°F) on a target that fills the field of view. Repeatability and measurement time were measured on a Glint Distance Sensor pointed at a fixed target 1.65&nbsp;m (5.4&nbsp;ft) away indoors. Your results depend on the target, its angle and the light around it.
