# ESP32 DAC using Embedded C (ESP-IDF)

## Project Overview

- **DAC Channels Used**:  
  - **DAC1** → GPIO25  
  - **DAC2** → GPIO26  

- **Resolution**: 8-bit (0–255 digital values)
- **Output Voltage Range**: 0V to Vref (typically 3.3V from VDD)
- **Framework**: ESP-IDF (Espressif IoT Development Framework)
- **Language**: Embedded C

## Description

ESP32 has two 8-bit DAC (Digital-to-Analog Converter) channels connected to:
- **Channel 1 (DAC1)** → GPIO25
- **Channel 2 (DAC2)** → GPIO26

Each DAC channel can convert a digital value in the range **0–255** to a corresponding analog output voltage. The analog voltage output is proportional to the digital value, and the formula for conversion is:
**out_voltage = Vref * digi_val / 255**


Where:
- `out_voltage` is the actual analog voltage generated on the DAC pin
- `Vref` is the reference voltage (usually equal to the board’s power supply: VDD)
- `digi_val` is the digital value you write to the DAC (between 0 and 255)

The output voltage is generated via the DAC peripheral and not suitable for high-current loads.

**References**
ESP-IDF DAC API Documentation: (https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/peripherals/dac.html)

**Code Explanation**
- #include <stdio.h>
Includes the standard C input/output header.
Enables the use of functions like printf() for printing to the console/serial monitor.

- #include "freertos/FreeRTOS.h"
Includes the FreeRTOS base header used in all ESP-IDF projects.
Required to use FreeRTOS APIs like task delays and task management.

- #include "driver/gpio.h"
GPIO driver header (not directly used in this code but often included by default for GPIO functions).
Can be removed here unless you're planning to use GPIO functions in the future.

- #include "driver/dac.h"
Header file for the legacy DAC driver.
Contains functions like dac_output_enable() and dac_output_voltage(), which control DAC operation.

-  dac_output_enable(DAC_CHAN_1);
Enables the DAC channel 1 (GPIO25).
This must be called before sending any analog value to the DAC.
DAC_CHAN_1 is a macro representing DAC channel 1.

- while loop
Creates an infinite loop that runs continuously.
Everything inside this block will execute repeatedly as long as the ESP32 is powered on.
First for loop: counts up from 0 to 255.
Represents the rising edge of the ramp wave — gradually increasing the DAC output.

- dac_output_voltage(DAC_CHAN_1, i);
Sends the value of i to DAC channel 1.
The DAC converts this 8-bit digital value to an analog voltage.
Analog output = (Vref * i) / 255, where Vref ≈ 3.3V.

- vTaskDelay(50 / portTICK_PERIOD_MS);
Delays the loop by 50 milliseconds.
vTaskDelay() is a FreeRTOS function that suspends the current task for a specified duration.
portTICK_PERIOD_MS converts milliseconds into system ticks.

## Summary
This code generates a triangular waveform by:
Increasing the DAC output from 0 to 3.3V (in steps)
Then decreasing it back to 0V
It repeats endlessly with a 50ms step delay





