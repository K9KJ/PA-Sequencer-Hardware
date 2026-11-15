# PA Sequencer Hardware

The EME Sequencer v0.2 is an ESP32-S3 control board that coordinates a low-noise amplifier (LNA), coaxial relays, and a power amplifier (PA) during transmit and receive transitions. It accepts a push-to-talk (PTT) signal and front-panel button inputs and provides six relay outputs for station control. Its onboard analog-to-digital converters read external thermistors, forward and reflected power detector signals from a directional coupler, and other analog signals.

## Firmware

The ESP32 firmware controls relay assignments, switching order, and delays. The firmware repository documents its ESP32-S3-DevKitM-1 pinout. Check the GPIO assignments against the schematic for your board revision before flashing. [Power Amplifier Sequencer firmware](https://github.com/brokebit/Power-Amplifier-Sequencer)

## Relay outputs

Each channel uses a 5 V Omron G6K-2F-Y signal relay and has an onboard LED that lights when its coil is driven. The matching two-pin connector brings out an isolated, normally open dry contact. The two terminals are open when the coil is off and connected when the coil is energized. The external circuit supplies any voltage switched by the contacts.

| Relay | Output connector | Coil indicator |
| --- | --- | --- |
| RLA1 | CN-R1 | D3 |
| RLA2 | CN-R2 | D4 |
| RLA3 | CN-R3 | D5 |
| RLA4 | CN-R4 | D6 |
| RLA5 | CN-R5 | D7 |
| RLA6 | CN-R6 | D8 |

All six outputs work the same way electrically. Depending on station wiring and firmware, they can key a PA, operate an external antenna changeover circuit, or switch other station control signals. Each connector exposes one contact pair from its relay. See the [relay schematic](https://github.com/brokebit/PA-Sequencer-Hardware/blob/main/relays.kicad_sch) and [Omron G6K datasheet](https://components.omron.com/eu-en/system/files/2025-12/datasheet_pdf/K106-E1.pdf).

## Power and control connectors

| Connector | Function |
| --- | --- |
| **CN-PWR1** | DC power input. Pin 1 is positive and pin 2 is ground. The board generates 5 V and 3.3 V internally. See the input range note below. |
| **CN-PTT1** | Transceiver PTT/SEND input. Pin 1 is signal and pin 2 is ground. The input has a divider designed around the 5 V SEND signal cited in the schematic. |
| **CN-RST1** | External ESP32 reset. A momentary switch across RESET and GND resets the controller. |
| **CN-BTN1 through CN-BTN6** | Six separate panel button inputs. Each connector has one signal pin and one ground pin for a momentary switch. Firmware assigns the button actions. |
| **CN-LCD1** | Serial display connector. Pins 1–4 are display power, board TX, board RX, and GND. |
| **J1** | Display supply jumper. Bridge pins 1–2 for 3.3 V or pins 2–3 for 5 V. The jumper selects display power. The serial lines connect directly to the ESP32. |
| **CN-I2C1** | Expansion connector for 3.3 V I²C peripherals. Pins 1–4 are 3.3 V, SDA, SCL, and GND. This bus also serves the onboard ADCs. |
| **CN-GPIO1** | Two ESP32 expansion signals. Pin 1 is GPIO10 and pin 2 is GPIO9. |
| **J3 and J4** | Alternate socket headers for a wider ESP32 board with the corresponding pinout. U1 is the main ESP32-S3-DevKitC position. |

**DC input range.** The [power schematic](https://github.com/brokebit/PA-Sequencer-Hardware/blob/main/Power.kicad_sch) specifies 11–20 V, while the [PCB silkscreen](https://github.com/brokebit/PA-Sequencer-Hardware/blob/main/ESP32-Sequencer-v0.2.kicad_pcb) reads 10–25 V. Confirm the intended range for your board revision before using a voltage outside 11–20 V.

## Sensor connectors

| Connector | Function |
| --- | --- |
| **CN-ADC1** | Four single-ended analog voltage inputs on the first ADC, designed for signals up to 5 V. Pins 1–4 are labeled A1–A4 on the PCB. |
| **J5** | Two more single-ended analog voltage inputs on the second ADC, also designed for signals up to 5 V. Pins 1 and 2 are labeled A3 and A4. |
| **J2** | Thermistor input labeled A2. Connect the thermistor between the signal terminal and GND. |
| **J6** | Thermistor input labeled A1. Connect the thermistor between the signal terminal and GND. |

The analog voltage inputs use board ground as their reference. The thermistor circuits are designed for 100 kΩ NTC thermistors at 25 °C with a beta of 3950 K. See the [ADC schematic](https://github.com/brokebit/PA-Sequencer-Hardware/blob/main/adc.kicad_sch).

For connector locations and wiring details, see the [PCB layout](https://github.com/brokebit/PA-Sequencer-Hardware/blob/main/ESP32-Sequencer-v0.2.kicad_pcb), [main schematic](https://github.com/brokebit/PA-Sequencer-Hardware/blob/main/ESP32-Sequencer-v0.2.kicad_sch), and [button schematic](https://github.com/brokebit/PA-Sequencer-Hardware/blob/main/Buttons.kicad_sch).
