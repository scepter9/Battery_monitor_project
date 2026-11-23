```markdown
# BattSense: Smart Battery Health Monitoring System

## Introduction

BattSense is an STM32-based battery monitoring system designed to measure and monitor important battery parameters, including voltage, current, and temperature.

The motivation behind this project was to develop a practical embedded system that could collect battery measurements and provide useful information about its operating condition. Rather than relying on ready-made development boards and monitoring modules, I wanted to design a custom PCB and understand how the different hardware components work together.

The project combines microcontroller programming, sensor interfacing, analogue signal measurement, power management, and custom PCB design. It was developed using KiCad, with the STM32F103C8 serving as the main processing unit.

## Design

### Why BattSense?

Batteries are used in several applications, from portable electronics to renewable energy systems and electric vehicles. Monitoring their operating parameters is important for understanding their condition and identifying potentially unsafe operating conditions.

I chose to build BattSense to explore how an embedded system can monitor these parameters through external sensing circuits and process the collected data using a microcontroller.

The project also provides an opportunity to gain practical experience in custom PCB design, component selection, sensor interfacing, and developing hardware around an STM32 microcontroller.

### System Overview

The system is built around three primary measurements:

- **Voltage**: Measures the battery's terminal voltage.
- **Current**: Measures the current flowing through an external test load.
- **Temperature**: Monitors battery temperature using an external NTC thermistor.

The STM32 processes the sensor readings and makes the measurement data available for display.

### Microcontroller: STM32F103C8

The STM32F103C8 was selected as the main controller because of its processing capability and range of integrated peripherals.

It is based on the ARM Cortex-M3 architecture and supports features such as:

- 12-bit ADC for analogue measurements.
- GPIO for interfacing with external components.
- I²C for communication with compatible peripherals.
- SPI and USART communication interfaces.
- SWD for programming and debugging.

The ADC is particularly important to this project because it allows the microcontroller to read analogue signals from the voltage measurement circuit and temperature sensor.

### Voltage Measurement

Battery voltage is measured using a resistive voltage divider.

The divider consists of two resistors connected in series across the battery terminals. It scales the battery voltage down to a level suitable for the STM32's ADC input.

This is necessary because the STM32 operates at 3.3V logic levels, and its ADC input must remain within the permitted voltage range.

The microcontroller reads the divided voltage and calculates the original battery voltage using the voltage divider ratio.

### Current Measurement

Current measurement is implemented using an INA219 current-sensing IC and an external shunt resistor.

The INA219 measures the small voltage difference across the shunt resistor. Using the resistance value of the shunt, the current flowing through the circuit can be calculated.

The shunt resistor connects to the PCB through a dedicated two-pin connector, which carries the differential sensing signals to the INA219.

An RC filter network is incorporated into the sensing path to help reduce unwanted high-frequency noise. Additionally, two TVS diodes are included at the shunt connector to provide protection against transient voltage disturbances.

For this project, the battery supplies an external test load, with the current flowing through the shunt resistor. This allows the system to monitor the current drawn during battery testing.

### Temperature Measurement

Battery temperature is measured using an external 10kΩ NTC thermistor.

Instead of mounting the thermistor directly on the PCB, a dedicated two-pin connector is provided for connecting the external temperature sensor.

The thermistor works with a fixed resistor to form a voltage divider. Since an NTC thermistor decreases in resistance as temperature increases, the voltage across the divider changes with temperature.

This analogue voltage is read by the STM32 ADC, and the microcontroller uses the thermistor's characteristics to estimate the temperature.

The connector arrangement allows the temperature sensor to be positioned closer to the battery being monitored.

### External LCD Display

The system will use an external LCD to display the battery measurements.

Rather than integrating the LCD directly onto the main PCB, it will be soldered separately and connected to the STM32 through jumper wires.

This arrangement keeps the main PCB design separate from the display module while allowing the microcontroller to communicate with the LCD and present the measured battery parameters.

### Power Supply

The PCB includes power regulation circuitry to provide the required operating voltages for the microcontroller and other electronic components.

The power section is designed to provide stable supply voltages while supporting the different components used throughout the system.

### PCB Layout

The PCB was designed in KiCad, with the components arranged according to their functions and electrical connections.

The layout brings together the microcontroller, voltage measurement circuit, current-sensing circuit, temperature measurement circuit, and power supply section.

The current-sensing connector includes an RC filter network and TVS protection components. A separate two-pin connector is provided for the external NTC thermistor.

Care was taken during component placement and routing to organise the different sections of the design and provide clear connections between the sensing circuits and the STM32.

The schematic and PCB layout are included in the project repository to make the hardware design available for inspection and further development.

## Hardware Components

The major components used in the design include:

| Component | Purpose |
|-----------|---------|
| STM32F103C8 | Main microcontroller |
| INA219 | Current measurement |
| External shunt resistor | Current sensing |
| RC filter network | Filtering noise in the current-sensing path |
| Two TVS diodes | Transient protection at the shunt connector |
| 47kΩ and 10kΩ resistors | Voltage divider |
| 10kΩ NTC thermistor | External temperature measurement |
| Fixed resistor | Thermistor voltage divider |
| Two-pin connector | External thermistor connection |
| Two-pin connector | Shunt sensing connection |
| Voltage regulators | Power supply regulation |
| External LCD | Displaying measurement readings |
| Jumper wires | Connecting the external LCD to the STM32 |

## How the System Works

The operation of BattSense can be summarised in the following stages:

1. **Voltage sensing**: The voltage divider scales down the battery voltage to a suitable level for the STM32 ADC.
2. **Current sensing**: The INA219 measures the voltage across the external shunt resistor to determine the current flowing through the test load.
3. **Signal conditioning and protection**: The RC filter network helps reduce noise in the current-sensing path, while the TVS diodes provide transient protection at the shunt connector.
4. **Temperature sensing**: The external NTC thermistor produces a temperature-dependent analogue voltage.
5. **Data processing**: The STM32 receives the sensor readings and processes them to obtain the corresponding voltage, current, and temperature values.
6. **Display**: The processed measurements are sent to the external LCD for presentation.

## Assembly and Documentation

The project was designed using KiCad. The repository contains the relevant hardware design files and supporting documentation.

The main design files include:

- KiCad schematic file.
- KiCad PCB layout file.
- KiCad project file.
- Schematic PDF.
- Bill of Materials (BOM) in CSV format.
- PCB layout and 3D render images.

These files make it possible to inspect the circuit, review component selections, modify the PCB, and continue development of the project.

## Lessons Learned

Working on BattSense provided practical experience in several areas of electronics and embedded systems.

Some of the key areas explored during the project include:

1. **STM32 Hardware Design**: Understanding the connections and supporting circuitry required when designing around an STM32 microcontroller.
2. **Analogue Measurements**: Using voltage dividers and ADC inputs to measure external electrical signals.
3. **Current Sensing**: Understanding how a shunt resistor and current-sensing IC work together.
4. **Signal Conditioning**: Incorporating RC filtering and TVS protection into external sensing connections.
5. **Sensor Interfacing**: Designing external sensor connections and reading temperature measurements using a microcontroller.
6. **PCB Design**: Translating a circuit schematic into a physical PCB layout using KiCad.

## Next Steps

The project provides a foundation for further development. Planned improvements include:

1. **Firmware Development**: Implementing the STM32 firmware for acquiring, processing, and displaying battery measurements.
2. **LCD Integration**: Connecting the external LCD using jumper wires and implementing the display interface.
3. **Calibration**: Comparing voltage, current, and temperature readings against appropriate reference instruments to evaluate measurement accuracy.
4. **Battery Testing**: Carrying out controlled battery tests and analysing the collected measurements.
5. **Further Improvements**: Exploring additional battery analysis features and improving the overall monitoring system.

## Project Files

The complete hardware design and supporting documentation are available in this repository.

- **KiCad Project**: `Hardware/KiCad/`
- **Schematic PDF**: `Hardware/Documentation/`
- **Bill of Materials**: `Hardware/Documentation/`
- **PCB Images**: `images/`
- **Fabrication Files**: `Hardware/Fabrication/`
```