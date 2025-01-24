---
title: "Steren RGBWW Desk Lamp"
date-published: 2025-01-23
type: light
standard: global
board: bk72xx
made-for-esphome: false
difficulty: 2
---

# Steren 12W Multicolor LED Wi-Fi Desk Lamp

The **Steren RGBWW Desk Lamp** is a smart desk lamp with a sleek design, offering RGB color control, dimming, and scene modes. It integrates with Tuya/Smart Life for seamless app control and smart home automation. It is also compatible with **ESPHome** after flashing firmware, making it a versatile addition to DIY smart home setups.

## Device Details

- **Model**: SHOME-LAM
- **Board**: BK7231 (wb3l variant)
- **Power**: 12W

## Features

- **12W LED Power**: Bright and energy-efficient.
- **RGB Multicolor**: Choose from millions of colors.
- **Dimmable**: Adjustable brightness to fit any mood or task.

## ESP-Compatibility

This lamp uses a BK7231, making it flashable with **ESPHome** .

## Images

![Placeholder for Lamp Image - Front](image-placeholder-front.png)
_Front view of the lamp showing its sleek design._

![Placeholder for Lamp Image - Back](image-placeholder-back.png)
_Back view showing connectivity and power input._

![Placeholder for Circuit Board](image-placeholder-circuit-board.png)
_Internal circuit board showcasing the chipset._

## Pinout and Configuration

### GPIO Mapping

| GPIO | Function   | Description           |
| ---- | ---------- | --------------------- |
| P8   | Red        | RGB LED Red Control   |
| P26  | Blue       | RGB LED Blue Control  |
| P24  | Green      | RGB LED Green Control |
| P6   | Warm White | White LED Control     |
| P7   | Cold White | White LED Control     |

## Flashing Instructions

1. Disassemble the lamp to access the PCB.
2. Identify the microcontroller (e.g., WB2L/BK7231).
3. Flash ESPHome using the configuration below.

### Final YAML Configuration

```yaml
substitutions:
  name: bolita
  friendly_name: "Bolita"

esphome:
  name: ${name}
  name_add_mac_suffix: false
  friendly_name: ${friendly_name}

bk72xx:
  board: wb3l

# Enable logging
logger:

# Enable Home Assistant API
api:
  encryption:
    key: "Mc0aA="

ota:
  - platform: esphome
    password: "4f84b5a8"

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: ${friendly_name} "Fallback Hotspot"
    password: ${friendly_name}123

captive_portal:

text_sensor:
  - platform: wifi_info
    ip_address:
      name: "ESP IP Address"

sensor:
  - platform: uptime
    name: "Uptime"
    unit_of_measurement: minutes
    filters:
      - lambda: return x / 60.0;

  - platform: wifi_signal
    name: "Signal"
    update_interval: 60s

output:
  - platform: libretiny_pwm
    id: red
    pin: P8
  - platform: libretiny_pwm
    id: blue
    pin: P26
  - platform: libretiny_pwm
    id: green
    pin: P24
  - platform: libretiny_pwm
    id: warm_white
    pin: P6
  - platform: libretiny_pwm
    id: cold_white
    pin: P7

light:
  - platform: rgbww
    name: "Light"
    id: rgb_light
    restore_mode: ALWAYS_OFF
    red: red
    green: green
    blue: blue
    warm_white: warm_white
    cold_white: cold_white
    cold_white_color_temperature: 6536 K
    warm_white_color_temperature: 2000 K
    color_interlock: True
    effects:
      - random:
          transition_length: 2.5s
          update_interval: 3s
      - random:
          name: Random Slow
          transition_length: 10s
          update_interval: 5s
      - pulse:
          name: "Fast Pulse"
          transition_length: 0.5s
          update_interval: 0.5s
          min_brightness: 0%
          max_brightness: 100%
      - pulse:
          name: "Slow Pulse"
          transition_length: 500ms
          update_interval: 2s
      - pulse:
          name: "Asymmetrical Pulse"
          transition_length:
            on_length: 1s
            off_length: 500ms
          update_interval: 1.5s
      - pulse:
          name: "Apple Breath"
          transition_length:
            on_length: 2.5s
            off_length: 1.5s
          update_interval: 4.5s
          min_brightness: 20%
          max_brightness: 40%
      - flicker:
          alpha: 90%
          intensity: 3%
```
