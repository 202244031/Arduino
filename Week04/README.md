# Dust Density Measurement with LCD Display

This project uses an Arduino, a dust sensor (such as the GP2Y1010AU0F), and an I2C-enabled LCD to measure and display the dust density in the air. The dust sensor provides an analog voltage that corresponds to the dust level, and the data is shown on the LCD screen.

## Components Required

- Arduino board (e.g., Arduino Uno)
- Dust sensor (e.g., GP2Y1010AU0F)
- I2C-enabled LCD (16x2)
- Jumper wires
- Breadboard (optional)

## Wiring Diagram

1. **Dust Sensor (GP2Y1010AU0F)**:
   - **VCC**: 5V
   - **GND**: GND
   - **Vo (Analog Output)**: Connect to A0 (Arduino Analog Pin)
   - **LED (Control Pin)**: Connect to Digital Pin 7 (Arduino)

2. **I2C LCD**:
   - **VCC**: 5V
   - **GND**: GND
   - **SDA**: A4 (Arduino)
   - **SCL**: A5 (Arduino)

## Libraries Required

The following libraries are needed for the project to run:

- **Wire.h**: This library is required for communication between the Arduino and the I2C LCD.
- **LiquidCrystal_I2C.h**: This library allows the Arduino to interface with the I2C-enabled LCD.

To install the libraries:

1. Open Arduino IDE
2. Go to **Sketch** -> **Include Library** -> **Manage Libraries**
3. Search for `LiquidCrystal_I2C` and `Wire`, then install them.

## Code Explanation

### Setup
- The LCD is initialized and a welcome message is displayed for 2 seconds.
- The dust sensor’s LED control pin (Pin 7) is set as an output.
- The analog input pin (A0) is used to read the sensor's output voltage.

### Loop
- The LED on the dust sensor is turned off for 280 microseconds to stabilize.
- The analog value from the sensor is read and then converted to a voltage.
- The dust density is calculated based on the formula:
  ```cpp
  dustDensity = (Voltage - 0.5) / 0.005;

