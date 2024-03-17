---
title: Introduction to ZMK
sidebar_label: Introduction
slug: /
---

ZMK Firmware is an open source (MIT) keyboard
firmware built on the [Zephyr™ Project](https://zephyrproject.org/) Real Time Operating System (RTOS). ZMK's goal is to provide a modern, wireless, and powerful firmware free of licensing issues.

## Features

ZMK is currently missing some features found in other popular firmware. This table compares the features supported by ZMK, BlueMicro and QMK:

| Legend: | ✅ Supported | 🚧 Under Development | 💡 Planned |
| :------ | :----------- | :------------------- | :--------- |

| **Feature**                                                                                                                        | ZMK | BlueMicro | QMK |
| ---------------------------------------------------------------------------------------------------------------------------------- | :-: | :-------: | :-: |
| Low Latency BLE Support                                                                                                            | ✅  |    ✅     |     |
| Multi-Device BLE Support                                                                                                           | ✅  |           |     |
| [USB Connectivity](behaviors/outputs.md)                                                                                           | ✅  |    ✅     | ✅  |
| User Configuration Repositories                                                                                                    | ✅  |           |     |
| Split Keyboard Support                                                                                                             | ✅  |    ✅     | ✅  |
| [Keymaps and Layers](behaviors/layers.md)                                                                                          | ✅  |    ✅     | ✅  |
| [Hold-Tap](behaviors/hold-tap.mdx) (which includes [Mod-Tap](behaviors/mod-tap.md) and [Layer-Tap](behaviors/layers.md#layer-tap)) | ✅  |    ✅     | ✅  |
| [Tap-Dance](behaviors/tap-dance.mdx)                                                                                               | ✅  |  ✅[^2]   | ✅  |
| [Keyboard Codes](codes/index.mdx#keyboard)                                                                                         | ✅  |    ✅     | ✅  |
| [Media](codes/index.mdx#media-controls) & [Consumer](codes/index.mdx#consumer-controls) Codes                                      | ✅  |    ✅     | ✅  |
| [Encoders](features/encoders.md)                                                                                                   | ✅  |    ✅     | ✅  |
| [Display Support](features/displays.md)[^1]                                                                                        | 🚧  |    🚧     | ✅  |
| [RGB Underglow](features/underglow.md)                                                                                             | ✅  |    ✅     | ✅  |
| [Backlight](features/backlight.mdx)                                                                                                | ✅  |    ✅     | ✅  |
| One Shot Keys                                                                                                                      | ✅  |    ✅     | ✅  |
| [Combo Keys](features/combos.md)                                                                                                   | ✅  |           | ✅  |
| [Macros](behaviors/macros.md)                                                                                                      | ✅  |    ✅     | ✅  |
| Mouse Keys                                                                                                                         | 🚧  |    ✅     | ✅  |
| Low Active Power Usage                                                                                                             | ✅  |           |     |
| Low Power Sleep States                                                                                                             | ✅  |    ✅     |     |
| [Low Power Mode (VCC Shutoff)](behaviors/power.md)                                                                                 | ✅  |    ✅     |     |
| Battery Reporting                                                                                                                  | ✅  |    ✅     |     |
| Shell over BLE                                                                                                                     | 💡  |           |     |
| Realtime Keymap Updating                                                                                                           | 💡  |           | ✅  |
| AVR/8 Bit                                                                                                                          |     |           | ✅  |
<<<<<<< HEAD
| [Wide Range of ARM Chips Supported](https://docs.zephyrproject.org/3.5.0/boards/index.html)                                        | ✅  |           |     |
=======
| [Wide Range of ARM Chips Supported](https://docs.zephyrproject.org/3.2.0/boards/index.html)                                        | ✅  |           |     |
>>>>>>> send-string-behavior/send-string-behavior

[^2]: Tap-Dances are limited to single and double-tap on BlueMicro
[^1]: OLEDs are currently proof of concept in ZMK.

## Code Of Conduct

Please note that this project is released with a
[Contributor Code of Conduct](https://www.contributor-covenant.org/version/2/0/code_of_conduct/).
By participating in this project you agree to abide by its terms.
