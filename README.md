# Covid Health Gadget

> **Contactless fever & oxygen‑level monitor built during the COVID‑19 pandemic**

## Why we built it

During the early waves of COVID‑19, hospitals were overwhelmed and many people needed a quick way to check for two key indicators of infection severity—fever and blood‑oxygen saturation—without visiting a clinic. This gadget combines a non‑contact temperature sensor with a fingertip pulse‑oximeter so anyone can monitor their status at home or in community checkpoints.

## Key features

* 🌡️ **Non‑contact body‑temperature** measurement via MLX90614 (I²C, ±0.2 °C accuracy)
* 💓 **Pulse‑rate & SpO₂** measurement via MAX30100
* 🚦 **Traffic‑light indicator** (green / yellow / red LED) gives at‑a‑glance status
* 🔋 **Rechargeable Li‑ion battery**, \~8 h runtime on a single charge
* 📝 **Serial & Bluetooth data logging** (optional module)
* 🖨️ **3D‑printed enclosure** for pocket‑size portability

## Hardware bill of materials

| Qty | Component                                   | Notes                               |
| --- | ------------------------------------------- | ----------------------------------- |
| 1   | MLX90614 IR thermometer                     | GND, VCC, SDA, SCL                  |
| 1   | MAX30100 pulse‑oximetry sensor              | GND, VCC (1.8–3.3 V), SDA, SCL, INT |
| 1   | Microcontroller (Arduino Nano / ESP32)      | I²C @ 100 kHz                       |
| 1   | 128×64 OLED (SSD1306) *or* 16×2 LCD         | I²C or parallel                     |
| 3   | LEDs (red, yellow, green) + 220 Ω resistors | Status indication                   |
| 1   | Buzzer                                      | Audible fever alert                 |
| 1   | TP4056 + 18650 Li‑ion cell                  | Power management                    |
| –   | 3D‑printed enclosure                        | STL in `/enclosure/`                |

## Schematic

```
[Insert Fritzing image here]
```

## Firmware

Source lives in `firmware/arduino/`. Upload **CovidHealthGadget.ino** with Arduino IDE 1.8+:

```bash
git clone https://github.com/<your‑user>/covid-health-gadget.git
cd covid-health-gadget/firmware/arduino
arduino --upload CovidHealthGadget.ino
```

### Required libraries

* **Adafruit MLX90614** [https://github.com/adafruit/Adafruit-MLX90614-Library](https://github.com/adafruit/Adafruit-MLX90614-Library)
* **MAX30100lib** [https://github.com/oxullo/Arduino-MAX30100](https://github.com/oxullo/Arduino-MAX30100)
* **Adafruit SSD1306** & **Adafruit GFX** (if using OLED)

## How it works

1. **MLX90614** samples forehead temperature once per second.
2. **MAX30100** samples IR & red reflectance at 100 Hz to compute SpO₂ and pulse.
3. MCU fuses the data and categorises risk:

   * **Green:** Temp < 37.5 °C *and* SpO₂ ≥ 95 %
   * **Yellow:** Temp 37.5–38.0 °C *or* SpO₂ 90–94 %
   * **Red:** Temp > 38.0 °C *or* SpO₂ < 90 %
4. Results are shown on the display and sent over serial/BLE.

## Usage

1. Power on (long‑press power button).
2. Hold the device \~5 cm from the centre of your forehead.
3. Lightly place your index finger on the MAX30100 window.
4. Keep still for \~10 s until readings stabilise.
5. Follow the LED indicator and displayed advice. **Seek medical help if the indicator is red.**

> **Disclaimer:** This is a hobby project and not a certified medical device. Always consult a healthcare professional for an official diagnosis.

## Project structure

```
/firmware/arduino            → Arduino sketch & libs
/enclosure/                  → STL files for 3D‑printed case
/hardware/schematic          → KiCad & PNG exports
/doc/                        → Datasheets & application notes
```

## Roadmap

* BLE + mobile companion app
* Cloud logging via MQTT/ThingsBoard
* Slimmer ergonomic enclosure
* Automatic firmware OTA updates (ESP32 target)

## License

MIT © 2025 Reyansh

## Acknowledgements

* Community volunteers who helped test early prototypes
* Frontline healthcare workers worldwide
* Open‑source library authors
