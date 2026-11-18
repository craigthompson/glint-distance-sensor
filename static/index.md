---
layout: default
permalink: /
tagline: Set up your sensor in a couple of minutes
---

# Quick start

A Wi-Fi distance sensor for [Home Assistant](https://www.home-assistant.io/): salt and water tank levels, bins, parking, or anything else where knowing a distance up to about 2 m is useful. This guide takes you from unboxing to readings in Home Assistant. Building your own, or reinstalling? See [Install firmware]({{ site.baseurl }}/firmware/). For range, precision and everything it can do, see [Features & specifications]({{ site.baseurl }}/specs/).

## What you need

- Your Glint Distance Sensor and a USB-C cable
- A USB power adapter (any phone charger works)
- [Home Assistant](https://www.home-assistant.io/) on the same network
- A **2.4 GHz** Wi-Fi network and its password. Most routers have one; 5 GHz-only networks don't work.
- A phone, to connect the sensor to Wi-Fi

## 1. Plug it in

Plug the sensor into USB power. After a second the light starts **blinking slowly** (1 second on, 1 second off). That means it's ready for Wi-Fi setup.

## 2. Connect it to Wi-Fi

1. On your phone, open Wi-Fi settings and join **Glint Distance Sensor Setup**. It has no password.
2. A setup page usually opens by itself. If it doesn't, open your phone's web browser: the setup page should appear. If it still doesn't, go to `http://192.168.4.1`.
3. Tap your network in the list and enter its password.

<div class="shots"><figure><img src="{{ site.baseurl }}/images/setup-networks.jpg" width="360" height="394" alt="Setup page listing nearby Wi-Fi networks" loading="lazy"><figcaption>Tap your network</figcaption></figure><figure><img src="{{ site.baseurl }}/images/setup-password.png" width="360" height="425" alt="Setup page with network name and password fields and a Connect button" loading="lazy"><figcaption>Enter its password and tap Connect</figcaption></figure></div>

The light **blinks fast** while it connects, then **blinks twice and pauses** once it's on your Wi-Fi. Your phone reconnects to your usual network by itself.

<div class="note"><strong>Prefer Bluetooth?</strong> If Home Assistant has Bluetooth within range of the sensor, it shows <em>Improv via BLE</em> under <strong>Settings → Devices &amp; services → Discovered</strong>. Click <strong>Add</strong>, then type your Wi-Fi network's name and password exactly.</div>

<div class="shots"><figure><img src="{{ site.baseurl }}/images/ha-discovered-improv.png" width="360" height="209" alt="Home Assistant Discovered card: glint-807298, Improv via BLE, with an Add button" loading="lazy"><figcaption>Improv via BLE under Discovered</figcaption></figure><figure><img src="{{ site.baseurl }}/images/ha-improv-wifi.png" width="360" height="298" alt="Connect to WiFi dialog asking for network name and password" loading="lazy"><figcaption>Enter your Wi-Fi details</figcaption></figure></div>

## 3. Add it to Home Assistant

1. Home Assistant finds the sensor within a minute or so. Go to **Settings → Devices & services**. Under **Discovered**, find **Glint Distance Sensor** (ESPHome) and click **Add**.
2. Click **Submit** to confirm, then choose an area for the sensor if you like.

<div class="shots"><figure><img src="{{ site.baseurl }}/images/ha-settings-devices.png" width="360" height="105" alt="Home Assistant Settings with Devices &amp; services highlighted" loading="lazy"><figcaption>Settings → Devices &amp; services</figcaption></figure><figure><img src="{{ site.baseurl }}/images/ha-discovered-esphome.png" width="360" height="209" alt="Home Assistant Discovered card: Glint Distance Sensor 807298, ESPHome, with Ignore and Add buttons" loading="lazy"><figcaption>Glint Distance Sensor under Discovered</figcaption></figure></div>

The light starts **breathing slowly**: the sensor is set up. It takes its first measurement a few seconds later.

Nothing to install: ESPHome support is built into Home Assistant. You don't need the ESPHome Device Builder add-on. If the sensor doesn't show up, see [Home Assistant doesn't find the sensor](#home-assistant-doesnt-find-the-sensor).

## 4. Mount it

The sensor measures the distance to whatever is straight in front of the small dark window on its front, up to about **2 metres** (6.6 ft) for light-coloured surfaces and about 80 cm (31 in) for dark ones. Anything farther away reads as out of range, so Distance shows Unknown.

- **Point it straight at the surface** you want to measure. For a tank or bin, mount it under the lid looking down.
- **Remove any protective film** from the window, and don't put anything over it, not even clear plastic.
- **Keep the path clear.** Walls, pipes or the edge of a narrow opening in the way give wrong readings.
- **Avoid direct sunlight** on the sensor or the target. Strong sunlight shortens the range and adds noise.
- Dark, shiny or angled surfaces reflect less light, so the range is shorter.

To check the position, press **Measure Now** on the sensor's device page and look at **Distance**.

## 5. Show a percentage (optional)

**Percentage** turns the distance into a level from 0 to 100%, for example how full a salt tank is.

1. With the tank **empty**, press **Measure Now** and note the **Distance**. Enter it as **0% Distance**.
2. With the tank **full**, do the same and enter the reading as **100% Distance**.

It works either way round: full can be the shorter or the longer distance. Both settings take 0 to 2000 mm; 0 means not set, and **Percentage** stays Unknown until both are set. Readings beyond either end show as 0% or 100%.

<div class="shots"><figure><img src="{{ site.baseurl }}/images/device-configuration.png" width="360" height="437" alt="Configuration card on the sensor device page with 0% Distance, 100% Distance and other settings" loading="lazy"><figcaption>0% and 100% Distance under Configuration</figcaption></figure></div>

## 6. Adjust it to your needs

All on the sensor's device page in Home Assistant:

<div class="shots"><figure><img src="{{ site.baseurl }}/images/device-controls-sensors.png" width="360" height="299" alt="Controls and Sensors cards: Measure Now, Scheduled Measurements, Distance 5.2 in and Percentage 85%" loading="lazy"><figcaption>Controls and readings on the device page</figcaption></figure></div>

| Setting | What it does |
|---|---|
| **Scheduled Measurements** | Turn measuring on the interval off and on. While off, only **Measure Now** measures. |
| **Measurement Interval** | How often it measures. Default every 30 minutes; anything from 0.5 seconds to 24 hours, in steps of 0.1 seconds. |
| **Samples per Measurement** | Readings averaged per measurement (1–10, default 5). More is smoother. |
| **Status LED** | Turn the light off or dim it. Picking the **None** effect makes it light steadily. |
| **Measure Now** | Measures right away. |

**Measurement Result** tells you how the last measurement went: **OK**, **Nothing in range**, or **Sensor not responding**. **Sensor Problem** turns on if the sensor stops responding, which makes it a good trigger for a "needs attention" notification.

## Extend Bluetooth range (optional)

The sensor can also work as a Bluetooth proxy, so Home Assistant hears Bluetooth devices near it: sensors, locks, and phones or tags for presence tracking.

1. On the sensor's device page, turn on **Bluetooth Proxy**.
2. Home Assistant adds it under **Settings → Devices & services → Bluetooth** by itself.
3. For tracking, use **Private BLE Device** (phones and watches), **iBeacon Tracker**, or Bermuda from HACS for room-level presence.

The proxy shares the sensor's radio with Wi-Fi, so it slows the sensor's Wi-Fi. If the sensor starts dropping off, turn the proxy off or move the sensor closer to your Wi-Fi.

## Status light

| Light | Meaning |
|---|---|
| Slow blink (1 s on, 1 s off) | Needs Wi-Fi setup. |
| Fast blink | Connecting to Wi-Fi. |
| 2 blinks, pause | On Wi-Fi, but not connected to Home Assistant. Add it in Home Assistant, or check that Home Assistant is running. |
| 3 blinks, pause | Sensor problem: the distance sensor isn't responding. |
| Very fast flicker | Counting power cycles for a factory reset. |
| Solid on | Installing a firmware update. Don't unplug it. |
| Slow breathing | Working normally. It flashes briefly while measuring. |

With the **Status LED** turned off in Home Assistant, the light stays dark except for the setup, Wi-Fi, reset and update patterns.

## Updates

New firmware shows up in Home Assistant as an update for the sensor, like any other update. Install it from **Settings → Updates** or the sensor's device page. The light stays on while it installs, then the sensor restarts.

## Troubleshooting

### The light keeps blinking fast

The sensor can't join your Wi-Fi. Check that it's within range and that the network is 2.4 GHz. If you entered the wrong password, or your network has changed, wait about a minute and a half: the **Glint Distance Sensor Setup** hotspot comes back, and you can enter the details again.

### I can't find the setup hotspot

Make sure the light is blinking slowly. If it's doing something else, the sensor already has Wi-Fi details; do a factory reset (below) to start over.

### Home Assistant doesn't find the sensor

Check that the light blinks twice and pauses: the sensor is on Wi-Fi and waiting. Home Assistant finds it automatically only if it's on the same network as the sensor. A separate network for smart devices (VLAN), or Home Assistant running in Docker without host networking, blocks discovery.

To add it by hand, go to **Settings → Devices & services → Add integration → ESPHome** and enter the sensor's name, e.g. `glint-distance-sensor-a1b2c3.local`. Your router's list of connected devices shows the full name.

### Home Assistant asks for an IP address

It found the sensor but couldn't reach it. Leave the address it filled in, wait a few seconds and click **Submit** again. If it keeps happening, move the sensor closer to your Wi-Fi router or access point.

### Distance shows Unknown

Right after the sensor starts, Distance is Unknown until the first measurement, a few seconds later. If it stays Unknown, **Measurement Result** says why.

<img class="screenshot" src="{{ site.baseurl }}/images/measurement-result-nothing-in-range.png" width="406" height="44" alt="Home Assistant showing Measurement Result: Nothing in range">

**Nothing in range** means the sensor is working but can't measure what it sees:

- The target is too far: more than about 2 m (6.6 ft), or 80 cm (31 in) if it's dark. A sensor lying on a desk or floor pointing at the ceiling is usually out of range.
- The target is very dark, shiny or at a steep angle.
- Something is covering the window, like protective film or tape, or the target is closer than about 3 cm (1.2 in).

To check, hold your hand about 30 cm (1 ft) in front of the sensor and press **Measure Now**. If Distance shows a value, the sensor is fine: move it closer to the target or point it straight at the surface.

**Signal Strength** helps tell these apart. It's hidden by default: on the sensor's device page, under **Diagnostic**, click the **disabled entities** link (for example **+2 disabled entities**), open **Signal Strength**, and enable it in its settings. A strong signal means something reflects the laser but is out of range or too close; a weak one means there's nothing in view.

**Sensor not responding** means the distance sensor itself isn't answering; see below.

### Percentage shows Unknown

Set both **0% Distance** and **100% Distance** to different values (see [Show a percentage](#5-show-a-percentage-optional)). It's also Unknown while Distance is Unknown.

### Sensor Problem is on

Measurement Result shows **Sensor not responding**, and the light blinks three times and pauses. The sensor resets its distance sensor and restarts by itself, up to three times. If the problem stays, unplug the sensor for a few seconds and plug it back in. If it still doesn't recover, the distance sensor may be damaged or loose inside.

## Factory reset

To erase the Wi-Fi details and all settings, for example to give the sensor to someone else:

- **Unplug and replug it 5 times in a row**, plugging it back in within 10 seconds each time. The light flickers very fast while it counts. After the fifth time it starts blinking slowly, ready for setup.
- Or, in Home Assistant, enable the sensor's **Factory Reset** button and press it.

Then delete the old device in Home Assistant and set the sensor up again from step 2.
