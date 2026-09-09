# Dhruvil's MP3 Player


---

## Documentation

For detailed technical documentation including component selection, design calculations, and system architecture:

📄 [Technical Documentation](docs/TECHNICAL_DOCS.md)

---

## Overview

This is a homemade portable MP3 player with a focus on clean audio, compact design, and learning analog-digital design for pcbs. This player plays MP3 files off an SD card and features a simple playback interface with volume control and track navigation, it does however support multiple files types such as wav,flac,midi,mp1-3.

The entire project is open-source — firmware, PCB design files, and 3D-printed case models are all available on GitHub.

---

## Features

- Compact 4-layer PCB measuring 60.72 mm X 86.85 mm, designed in Altium
- Separate Analog and Digitial LDO's to minimize analog noise and maintain audio quality
- TFT display showing track metadata: song name, album, artist, and duration
- USB charging with BQ24074 battery management IC
- Long battery life — runs for ~20 hours on a single charge with a 3.7V 2600mah battery, this will depend heavily on the screen brightness
- Custom 3D-printed enclosure designed in Fusion360

---

## Hardware

The player is built around an ESP32 microcontroller paired with a VS1053b audio DAC for MP3 decoding and audio ouput. A BQ24074 handles battery charging with power and data done over USB C-2.0, and all components are packed onto a 4-layer PCB with a SIG-GND-PWR-SIG stackup. A gpio expander PCF8575PWR is used to handle volume and playback switch inputs.

The PCB was designed in Altium. Components are hand-soldered and by a hot-air gun.

---

## Firmware

The ESP32 communicates with the SD card over SPI using standard libraries for FAT32 filesystem access. The VS1053b also speaks SPI, so firmware handles both interfaces efficiently.

The ESP32 also only uses 1 core to prolong battery life, more about this is listed in the documentation file.

Track metadata (song name, album, artist, duration) is parsed from the SD card and displayed on the TFT screen in real-time.

---

## Enclosure

The case is 3D-printed using Fusion360 and designed to snugly fit the PCB and battery. It uses a simple two-piece snap-fit design printed in PLA.

---

## Build Your Own

Everything you need to build your own is available in this repository:

- **PCB Design** — Altium project files, schematic, and Gerbers
- **Firmware** — ESP32 code, SPI drivers, and metadata parsing
- **Enclosure** — Fusion360 STL files for top and bottom case halves
- **Bill of Materials** — Full parts list with component sources
