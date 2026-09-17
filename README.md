# Radar Process — Ultrasonic Radar with Arduino & Processing

An Arduino-based ultrasonic radar system that sweeps a servo-mounted HC-SR04 sensor across a 15°–165° arc, streams angle/distance readings over serial, and visualizes them in real time as a classic green sweeping radar display using Processing.

## How It Works

1. **`RADAR.ino`** (Arduino sketch) sweeps a servo motor back and forth between 15° and 165°. At each degree step, it triggers an HC-SR04 ultrasonic sensor, calculates the distance to any detected object, and sends `angle,distance.` pairs over the serial port (9600 baud).
2. **`RADAR_PROCESS.pde`** (Processing sketch) reads the incoming serial data, parses the angle/distance values, and renders an animated radar display: a rotating sweep line, arc range rings (10/20/30/40 cm), and a highlighted blip wherever an object is detected within 40 cm.

## Hardware Requirements

- Arduino board (Uno/Nano/etc.)
- HC-SR04 ultrasonic distance sensor
- SG90 (or similar) servo motor
- Jumper wires / breadboard

### Wiring (as configured in `RADAR.ino`)

| Component | Arduino Pin |
|---|---|
| Ultrasonic Trig | Pin 10 |
| Ultrasonic Echo | Pin 11 |
| Servo signal | Pin 12 |

## Software Requirements

- [Arduino IDE](https://www.arduino.cc/en/software) with the built-in `Servo` library
- [Processing](https://processing.org/download) (3.x or later) with the `processing.serial` library (bundled by default)

## Project Structure

```
RADAR_PROCESS/
├── RADAR/
│   └── RADAR.ino            # Arduino sketch: servo sweep + ultrasonic distance measurement
└── RADAR_PROCESS.pde        # Processing sketch: radar visualization
```

## Getting Started

### 1. Upload the Arduino sketch
1. Wire up the HC-SR04 sensor and servo as described above.
2. Open `RADAR/RADAR.ino` in the Arduino IDE.
3. Select your board and port, then upload the sketch.

### 2. Run the Processing visualization
1. Open `RADAR_PROCESS.pde` in the Processing IDE.
2. **Update the serial port**: the sketch is hardcoded to `COM3`:
   ```java
   myPort = new Serial(this,"COM3", 9600);
   ```
   Change `"COM3"` to match your Arduino's serial port (e.g. `/dev/ttyUSB0` or `/dev/cu.usbmodemXXXX` on Linux/macOS — run `println(Serial.list())` in `setup()` to list available ports).
3. Run the sketch. A 1200×700 window will open showing the live radar sweep, with detected objects highlighted in red within a 40 cm range.

## Notes

- The 40 cm detection range and "Out of Range" / "In Range" labels are hardcoded in `RADAR_PROCESS.pde` and can be adjusted to suit your sensor/use case.
- The Arduino and Processing sketches must be run one after another (Arduino uploaded and running first) with matching serial port and baud rate (9600).

## Credits

Radar visualization logic based on the widely used Arduino/Processing HC-SR04 radar project pattern (originally referenced from andprof.com in code comments).
