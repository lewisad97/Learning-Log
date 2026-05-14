MIDAS-Modular Instrumentation and Data Acquisition system.

Hardware: (For the modules attached to the Arduino R4 microcontroller.)


Environmental sensor
BME280
Measures temperature, barometric pressure, and relative humidity simultaneously. Core environmental telemetry — the first sensor to wire up. 
Analogous to spacecraft thermal and atmospheric payload instruments.

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
INA219	I²C	SDA (A4), SCL (A5)	0x40–0x4F
VEML6070	I²C	SDA (A4), SCL (A5)	0x38 / 0x39
TSL2591	I²C	SDA (A4), SCL (A5)	0x29
NEO-6M GPS	UART	RX1/TX1 (pins 0,1)	N/A

Every sensor except the GPS shares the same two wires. That's the elegance of I²C — SDA and SCL run to all four modules in parallel, and the R4 selects which one it's talking to using the hex address. Wiring is simple: all SDA pins join together, all SCL pins join together, both lines connect to A4 and A5 on the R4.

The GPS is optional but genuinely worth adding. If you do any outdoor range testing with LoRa, having the node report its own position turns a radio experiment into a genuine tracking and telemetry demonstration. That's a significant step up in portfolio terms — you'd have a mobile node transmitting positional and environmental data to a fixed ground station, which is structurally identical to a small UAV or balloon telemetry system.

Wire them in this order when you get to Phase 2: BME280 first — simplest to verify, clean well-documented libraries. Then INA219. UV/light last. Each one gets tested in isolation before the next is added. The GPS comes in alongside or after LoRa since it needs outdoor sky visibility to get a fix.

Wiring:

I²C modules (BME280, MPU-6050, INA219, VEML6070, TSL2591)
Each of these has four pins: VCC, GND, SDA, and SCL. You run jumper wires from each module directly to the R4. All five share the same SDA and SCL pins — so those two lines will have multiple jumpers connecting to them, either daisy-chained or meeting at a breadboard rail. VCC goes to the R4's 3.3V pin, GND to any GND pin.
A small breadboard is useful here not for circuit building but purely as a wiring hub — it lets you connect multiple VCC wires to one 3.3V rail and multiple GND wires to one ground rail cleanly, rather than stacking jumpers directly on the R4 pins.

GPS (NEO-6M)
Four pins as well: VCC, GND, TX, RX. VCC on the NEO-6M is 3.3V or 5V tolerant depending on the module variant — check your specific board. TX on the GPS connects to RX on the R4 (pin 0), and RX on the GPS connects to TX on the R4 (pin 1). Straightforward jumper connections.
One thing to be aware of
The R4 operates at 3.3V logic on its I²C and UART pins. All the modules listed are 3.3V compatible, so no level shifting is needed — jumper wires connect directly. If you ever buy a module that says 5V only, you'd need a level shifter board between it and the R4, but none of the modules on this list require that.


I²C bus sharing: 
all five I²C sensors share the same two wires (SDA and SCL) — each is addressed individually by its unique hex address. 
This is one of I²C's key advantages for sensor-heavy designs. 
The GPS uses a separate UART port on the R4 (Serial1), keeping it isolated from the I²C bus entirely.
The LoRa module connects to the Raspberry Pi over SPI, not the R4.
