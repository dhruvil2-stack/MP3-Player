# MP3 Player - Technical Documentation

## Table of Contents

- [Power Management](#power-management)
- [Microcontroller Selection](#microcontroller-selection)
- [Audio Decoder Selection](#audio-decoder-selection)
- [Display Selection](#display-selection)
- [Enclosure Design](#enclosure-design)

---

## Power Management

### Battery Charging - BQ24074

The BQ24074 was selected as the battery charging IC due to its integrated power-path management and ability to support fast charging currents up to 1.5A. The charging current is configured using an external resistor connected to the ISET pin.

**Charging Current Calculation:**

$$I_{CHG} = \frac{K_{ISET}}{R_{ISET}}$$

$$R_{ISET} = \frac{K_{ISET}}{I_{CHG}} = \frac{890\ \text{A}\cdot\Omega}{0.80\ \text{A}} = 1.1125K\ \Omega \approx 1.1K\ \Omega$$

**Battery Specification:**

| Parameter | Value |
|-----------|-------|
| Chemistry | Lithium-Ion (Li-ion) |
| Nominal Voltage | 3.7V |
| Maximum Voltage | 4.2V |
| Capacity | 1500mAh |
| Charging Current | 0.80A (Fast Charge) |

**EN Pin Configuration:**

The EN1 and EN2 pins configure the input current limit based on the USB power source:

| EN1 | EN2 | Input Current Limit | Configuration |
|-----|-----|---------------------|---------------|
| 0   | 0   | USB 100mA (SDP)     | Not Used |
| 1   | 1   | USB 500mA           | Not Used |
| 0   | 1   | Adapter 1.3A        | **Selected** |
| 1   | 0   | Adapter 1.5A        | Not Used |

**Selected Configuration:**

The BQ24074 is configured with **EN1 = 0** and **EN2 = 1**, setting the input current limit to **0.80A**. This configuration was chosen to:

- Keep charging time to be relativelty fast while staying within the safe limits of charging a lipo
- Allow the BQ24074's power-path to manage battery charging and system power simultaneously
- Provide adequate headroom for system operation while charging

### Voltage Regulation - AP2112K-3.3V and AP2112K-1.8V

The AP2112K series LDOs were selected for voltage regulation based on the following specifications:

| Parameter | Value |
|-----------|-------|
| Dropout Voltage | 250mV typical at 600mA load |
| Output Current | Up to 600mA |
| Package | SOT-23-5 |
| Features | Enable pin, thermal protection, short-circuit protection |

**Power Rails:**

| Rail | Voltage | Regulator | Supply | Purpose |
|------|---------|-----------|--------|---------|
| 3.3V | 3.3V | AP2112K-3.3 | 3.7V Li-ion | ESP32, ST7735 display, VS1053b IOVDD/AVDD, SD card, logic levels |
| 1.8V | 1.8V | AP2112K-1.8 | 3.7V Li-ion | VS1053b core voltage (CVDD) |

**Selection Rationale:**

- Low dropout voltage (250mV typical) enables efficient operation from a 3.7V Li-ion battery while maintaining stable output voltages
- 600mA output capacity sufficient for all onboard components
- Enable pin allows controlled power sequencing and low-power standby modes
- Built-in over-temperature and short-circuit protection enhance system reliability

**VS1053b Power Requirements:**

The VS1053b requires two separate power supplies:

- **IOVDD (3.3V):** Powers the digital I/O pins and SPI interface
- **CVDD (1.8V):** Powers the internal core processor and DSP

The 1.8V rail is critical for proper VS1053b operation. The AP2112K-1.8 provides a stable, low-noise supply to ensure consistent audio decoding performance.

---

## Microcontroller Selection - ESP32

The ESP32 was selected as the main controller for the following reasons:

**Advantages:**

- Extensive library support for ST7735 TFT display drivers (ESP-IDF and Arduino frameworks)
- Multiple SPI peripherals allow simultaneous communication with VS1053b, SD card, and display without bus contention
- Sufficient processing power for metadata parsing, audio streaming control, and UI updates
- Large community and extensive documentation for troubleshooting
- Built-in Wi-Fi and Bluetooth (future expandability for wireless streaming or OTA updates)

**Pin Allocation:**

| Peripheral | Interface | ESP32 Pins |
|------------|-----------|------------|
| VS1053b | SPI | VSPI (MOSI, MISO, SCK, CS, DREQ, RST) |
| SD Card | SPI | VSPI or HSPI |
| ST7735 Display | SPI | VSPI (CS, DC, RST) |
| Buttons | GPIO | Multiple input pins |
| BQ24074 | GPIO | EN1, EN2, CHG status |

---

## Audio Decoder Selection - VS1053b

The VS1053b was selected as the audio codec based on the following factors:

**Key Specifications:**

| Parameter | Value |
|-----------|-------|
| Formats Supported | MP3, AAC, Ogg Vorbis, WMA, FLAC, WAV, MIDI |
| Interface | SPI |
| Headphone Output | Direct drive (no external amp required) |
| Core Voltage (CVDD) | 1.8V |
| I/O Voltage (IOVDD) | 3.3V |

**Selection Rationale:**

- Well-documented datasheet with comprehensive technical reference
- Successfully prototyped on a breakout board prior to PCB integration
- Integrated headphone amplifier drives headphones directly (load impedance ≥ 16Ω)
- Eliminates need for external amplifier, reducing current consumption
- Supports multiple audio formats for versatility

**Power Supply Requirements:**

The VS1053b requires two separate voltage rails:

- **CVDD (1.8V):** Core voltage for internal processor and DSP. Requires stable, low-noise supply.
- **IOVDD (3.3V):** Digital I/O voltage for SPI communication and control pins.
- **AVDD (3.3V):** Analog Power Supply 
The AP2112K-1.8 was selected for the CVDD rail due to its low dropout voltage and low-noise output, ensuring reliable audio decoding performance.

**Headphone Output:**

Typical earbuds have an impedance of 32Ω, which is well within the VS1053b's drive capability (minimum 16Ω load). The VS1053b uses a DC-coupled output with a 1.25V reference (GBUF), allowing direct headphone connection without coupling capacitors.

---

## Display Selection - ST7735

The ST7735 TFT display was selected for the following reasons:

**Key Specifications:**

| Parameter | Value |
|-----------|-------|
| Resolution | 160x128 |
| Interface | SPI |
| Color Depth | 65K colors (RGB565) |
| Operating Voltage | 3.3V |

**Selection Rationale:**

- Existing driver libraries available for ESP-IDF and Arduino
- SPI interface minimizes pin count
- Sufficient resolution for track metadata display
- Color capability enables clear UI elements

---

## Enclosure Design - 3D Printed

The enclosure was designed using Tinkercad for ease of iteration.

**Design Specifications:**

- Material: PLA
- Assembly: Snap-fit (no screws or adhesives required)
- Custom pockets for PCB and battery
- Low-profile design fits comfortably in hand
