---
layout: default
permalink: /
tagline: Set up your sensor in a couple of minutes
---

# Quick start

A Wi-Fi distance sensor for [Home Assistant](https://www.home-assistant.io/): salt and water tank levels, bins, parking, or anything else where knowing a distance up to about 2 m is useful. This guide takes you from unboxing to readings in Home Assistant. Building your own, or reinstalling? See [Install firmware]({{ site.baseurl }}/firmware/).

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

The light **blinks fast** while it connects, then **blinks twice and pauses** once it's on your Wi-Fi. Your phone reconnects to your usual network by itself.

<div class="note"><strong>Prefer Bluetooth?</strong> If Home Assistant has Bluetooth within range of the sensor, it shows <em>Improv via BLE</em> under <strong>Settings → Devices &amp; services → Discovered</strong>. Click <strong>Add</strong>, then type your Wi-Fi network's name and password exactly.</div>

## 3. Add it to Home Assistant

1. Home Assistant finds the sensor within a minute or so. Go to **Settings → Devices & services**. Under **Discovered**, find **Glint Distance Sensor** (ESPHome) and click **Add**.
2. Click **Submit** to confirm, then choose an area for the sensor if you like.

The light starts **breathing slowly**: the sensor is set up. It takes its first measurement a few seconds later.

Nothing to install: ESPHome support is built into Home Assistant. You don't need the ESPHome Device Builder add-on. If the sensor doesn't show up, see [Home Assistant doesn't find the sensor](#home-assistant-doesnt-find-the-sensor).

## 4. Mount it

The sensor measures the distance to whatever is straight in front of the small dark window on its front, up to about **2 metres**.

- **Point it straight at the surface** you want to measure. For a tank or bin, mount it under the lid looking down.
- **Keep the path clear.** Walls, pipes or the edge of a narrow opening in the way give wrong readings.
- **Avoid direct sunlight** on the sensor or the target. Strong sunlight shortens the range and adds noise.
- Dark, shiny or angled surfaces reflect less light, so the range is shorter.

To check the position, press **Measure Now** on the sensor's device page and look at **Distance**.

## 5. Show a percentage (optional)

**Percentage** turns the distance into a level from 0 to 100%, for example how full a salt tank is.

1. With the tank **empty**, press **Measure Now** and note the **Distance**. Enter it as **0% Distance**.
2. With the tank **full**, do the same and enter the reading as **100% Distance**.

It works either way round: full can be the shorter or the longer distance.

## 6. Adjust it to your needs

All on the sensor's device page in Home Assistant:

| Setting | What it does |
|---|---|
| **Scheduled Measurements** | Turn measuring on the interval off and on. While off, only **Measure Now** measures. |
| **Measurement Interval** | How often it measures. Default every 30 minutes; anything from 1 second to 24 hours. |
| **Samples per Measurement** | Readings averaged per measurement (1–10, default 5). More is smoother. |
| **Status LED** | Turn the light off or dim it. Picking the **None** effect makes it light steadily. |
| **Measure Now** | Measures right away. |

**Sensor Problem** turns on if the sensor stops responding, which makes it a good trigger for a "needs attention" notification.

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

Nothing is in range: the target is more than about 2 m away, or is very dark or at a steep angle. Move the sensor closer or point it straight at the surface, then press **Measure Now**.

### Sensor Problem is on

The sensor restarts its distance sensor by itself up to three times. If the problem stays, unplug the sensor for a few seconds and plug it back in.

## Factory reset

To erase the Wi-Fi details and all settings, for example to give the sensor to someone else:

- **Unplug and replug it 5 times in a row**, plugging it back in within 10 seconds each time. The light flickers very fast while it counts. After the fifth time it starts blinking slowly, ready for setup.
- Or, in Home Assistant, enable the sensor's **Factory Reset** button and press it.

Then delete the old device in Home Assistant and set the sensor up again from step 2.
