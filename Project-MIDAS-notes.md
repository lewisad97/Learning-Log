MIDAS-Modular Instrumentation and Data Acquisition system.

Hardware: (For the modules attached to the Arduino R4 microcontroller.)


Environmental sensor
BME280
Measures temperature, barometric pressure, and relative humidity simultaneously. Core environmental telemetry — the first sensor to wire up. 
Analogous to spacecraft thermal and atmospheric payload instruments.


6-axis IMU
MPU-6050
3-axis accelerometer and 3-axis gyroscope. Measures orientation, tilt, rotation rate, and vibration. 
Directly mirrors attitude determination sensors on real spacecraft — gives your node dynamic motion data rather than purely static readings.

Power monitor
INA219
Measures bus voltage, current draw, and calculates power consumption in real time. 
Sits in series with the power rail to the sensors. 
Equivalent to spacecraft electrical power system (EPS) telemetry — monitors node health and detects anomalous power draw.

UV and light sensor
VEML6070 + TSL2591
VEML6070 measures UV index (UVA band). TSL2591 measures visible and infrared light intensity in lux with very high dynamic range. 
Together they give your node a full optical environment profile — relevant to solar exposure monitoring on spacecraft.

GPS receiver
NEO-6M / NEO-M8N
Optional but high-value addition. Provides latitude, longitude, altitude, ground speed, and UTC time. 
If you deploy the node outdoors for a LoRa range test, GPS gives you real positional telemetry — directly analogous to spacecraft orbital state vector data. 
Adds significant portfolio value.

LoRa radio module
SX1276 / Ra-02
Phase 2 RF data link. Connects to the Raspberry Pi (not the R4 directly) over SPI. 
Transmits the OBC-packaged telemetry frames wirelessly to the ground station receiver at 868MHz. 
Licence-exempt in the UK at low power. Mirrors UHF CubeSat downlink architecture.

Module	Interface	R4 pins	I²C address
BME280	I²C	SDA (A4), SCL (A5)	0x76 or 0x77
MPU-6050	I²C	SDA (A4), SCL (A5)	0x68 or 0x69
INA219	I²C	SDA (A4), SCL (A5)	0x40–0x4F
VEML6070	I²C	SDA (A4), SCL (A5)	0x38 / 0x39
TSL2591	I²C	SDA (A4), SCL (A5)	0x29
NEO-6M GPS	UART	RX1/TX1 (pins 0,1)	N/A

I²C bus sharing: 
all five I²C sensors share the same two wires (SDA and SCL) — each is addressed individually by its unique hex address. 
This is one of I²C's key advantages for sensor-heavy designs. 
The GPS uses a separate UART port on the R4 (Serial1), keeping it isolated from the I²C bus entirely.
The LoRa module connects to the Raspberry Pi over SPI, not the R4.
