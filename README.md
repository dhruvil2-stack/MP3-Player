# Dhruvil's MP3 Player

A compact, open-source MP3 player built from the ground up. From PCB design to firmware to 3D-printed enclosure, everything is open-source and ready for you to build your own.

---

## Overview

This is a homemade portable MP3 player with a focus on clean audio, compact design, and hands-on learning. It plays MP3 files off a micro-SD card and features a simple playback interface with volume control and track navigation. Load up a card with your favourite tracks, plug it in, and you're good to go.

The entire project is open-source — firmware, PCB design files, and 3D-printed case models are all available on GitHub.

---

## Features

- Compact 4-layer PCB measuring 2x4 inches, designed in Altium
- Pi-filters on power rails to minimize analog noise and maintain audio quality
- Simple controls: volume up/down, play/pause, next/previous track
- TFT display showing track metadata: song name, album, artist, and duration
- USB charging with BQ24074 battery management IC
- Long battery life — runs for hours on a single charge
- Custom 3D-printed enclosure designed in Tinkercad

---

## Hardware

The player is built around an ESP32 microcontroller paired with a VS1053b audio DAC for MP3 decoding and high-quality audio output. A BQ24074 handles battery charging, and all components are packed onto a compact 4-layer PCB.

The PCB was designed in Altium. Components are hand-soldered, and the board includes careful power routing and filtering to keep the audio signal clean.

---

## Firmware

The ESP32 communicates with the SD card over SPI using standard libraries for FAT32 filesystem access. The VS1053b also speaks SPI, so firmware handles both interfaces efficiently.

Track metadata (song name, album, artist, duration) is parsed from the SD card and displayed on the TFT screen in real-time.

---

## Enclosure

The case is 3D-printed using Tinkercad and designed to snugly fit the PCB and battery. It uses a simple two-piece snap-fit design printed in PLA — no screws or adhesives required.

---

## Build Your Own

Everything you need to build your own is available in this repository:

- **PCB Design** — Altium project files, schematic, and Gerbers
- **Firmware** — ESP32 code, SPI drivers, and metadata parsing
- **Enclosure** — Tinkercad STL files for top and bottom case halves
- **Bill of Materials** — Full parts list with component sources

