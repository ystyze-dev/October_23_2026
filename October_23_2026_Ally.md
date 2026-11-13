# Aera: an embodied ally

The fan is an expressive companion: it notices a person, listens only when
invited, and responds with poetry. The browser page is a visual prototype; it
does not control an appliance or detect people automatically. The proposed
appliance is a four-motor, forward-facing propeller assembly in a spherical
containment cage, suspended from the ceiling, with a display and microphone
panel below it.

## Project definition

The concept has four matched propeller motors on a cross, with all propellers
facing forward. The intended control groups are **north + west** and
**south + east**. The intended motion is forward and back, created by changing
thrust between those groups. This is not yet a validated mechanism: thrust
imbalance may cause rotation, roll, vibration, or uncontrolled pendulum motion
instead of a predictable forward/back swing. A qualified mechanical and
controls engineer must establish whether this concept can be made stable,
guarded, and safe before motors or propellers are operated.

The propellers must be fully contained by a professionally designed,
fragment-resistant spherical cage; a decorative grille is not containment.
The cage, cross, motors, suspension, restraints, and support structure must be
engineered and approved as one overhead assembly. Do not test exposed drone
propellers, suspend an improvised assembly over people, or use propeller thrust
as the only means of preventing a fall or collision. If a safe thrust-based
motion system cannot be validated, use a certified oscillating fan or a
separately engineered movement mechanism instead.

The Arduino is the local motor-control processor, not a safety certification
or a ceiling/mains controller. A separately specified Raspberry Pi runs the
display, local sensor service, and user interface. An approved motor driver
for each motor and a hardware emergency stop / energy-isolation system are
required; the exact board, drivers, power system, and interfaces depend on
the selected motors and engineering review. Never connect a motor to an
Arduino GPIO or improvise a power stage.

The display, microphone, people-presence sensors, and controller may be
carried below the spherical cage only if the engineer approves that added
load, attachment, cable routing, and clearance. The browser page needs a
separate local service on the Pi to receive actual sensor readings and issue
approved low-voltage control requests. It must not control the motor power
directly.

## Intended interaction

1. A room-presence sensor detects occupancy, including a person sitting still.
   A separate range sensor may estimate distance and approach direction.
   These are interaction inputs only, not personnel-protection devices.
2. With no person present, an unknown/faulted sensor, controller fault, or
   emergency stop, the motor command is off. A prototype may simulate these
   states before any motor hardware is enabled.
3. If present, the fan may show an attentive state. The proposed 2 ft and
   8 ft thresholds remain configurable interaction choices, not safe
   operating distances. Do not use them to protect people from propellers.
4. A deliberate blow may dismiss the poem. Laughter may request a playful
   response only after validated, consent-based detection; audio loudness
   alone does not reliably identify either event.
5. Any motion command must remain disabled until a qualified engineer has
   validated the thrust/control concept, complete containment, and independent
   stop behavior. Do not make a sensor failure or reboot cause a motor start.

Presence, distance, breath, and laughter inputs are not safety-rated. The
assembly needs an independent physical emergency stop and a safe, engineered
means of isolating motor energy. Software cannot substitute for containment,
structural design, or a certified safety system.

## Parts list

The first list is for building and testing automatic detection, the display,
and the Pi-to-Arduino software link on a bench. It does not require motor
power or propellers. The second list is for a later motor-control phase; its
power and driver parts must match the exact motors and drivers you select.

### Buy for the sensor and software prototype

These are specific example models so the prototype can be assembled without
choosing a motor system. Check regional availability and connector revisions
before checkout.

| Qty | Purchase this item | Why / notes |
|---:|---|---|
| 1 | [Raspberry Pi 5, 4 GB — Amazon search](https://www.amazon.com/s?k=Raspberry+Pi+5+4GB) | Local web UI and sensor application. |
| 1 | [Official Raspberry Pi 27 W USB-C Power Supply — Amazon search](https://www.amazon.com/s?k=official+Raspberry+Pi+27W+USB-C+power+supply+US) | Dedicated Pi supply; not for motors. |
| 1 | [Official Raspberry Pi Case for Raspberry Pi 5 — Amazon search](https://www.amazon.com/s?k=official+Raspberry+Pi+5+case+active+cooling) | Includes active cooling. |
| 1 | [SanDisk High Endurance microSDXC, 64 GB — Amazon search](https://www.amazon.com/s?k=SanDisk+High+Endurance+64GB+microSDXC) | Pi OS and project storage. |
| 1 | [Arduino Mega 2560 Rev3 — Amazon search](https://www.amazon.com/s?k=Arduino+Mega+2560+Rev3+official) | USB-serial prototype controller; enough pins to model four motor channels. |
| 1 | [USB-A to USB-B data cable, 1 m — Amazon search](https://www.amazon.com/s?k=USB+A+to+USB+B+data+cable+1m) | Connects the Mega's USB-B port to a Pi USB-A port. Make sure it is a data cable. |
| 1 | [Hi-Link HLK-LD2410C 24 GHz mmWave presence sensor — Amazon search](https://www.amazon.com/s?k=Hi-Link+HLK-LD2410C+24GHz+presence+sensor+module) | Automatic room-presence sensing, including a stationary-person test. Buy the complete module/breakout, not the bare sensor IC. |
| 1 | [Adafruit CP2104 Friend USB-to-Serial Breakout, PID 3309 — Amazon search](https://www.amazon.com/s?k=Adafruit+CP2104+Friend+USB+to+Serial+3309) | Connects the LD2410C UART to Pi USB at 3.3 V logic. Use the sensor's documented power input and verify the particular module's UART levels before connecting. |
| 1 | [Regulated 5 V USB power adapter, 1 A or higher — Amazon search](https://www.amazon.com/s?k=5V+USB+power+adapter+1A+UL+listed) and [LD2410C power lead — Amazon search](https://www.amazon.com/s?k=HLK-LD2410C+power+cable) | Dedicated sensor power if required by the board version. Confirm the exact module's supply and connector; do not power it from the CP2104's 3.3 V output. |
| 1 | [Adafruit VL53L1X Time of Flight Distance Sensor, PID 3967 — Amazon search](https://www.amazon.com/s?k=Adafruit+VL53L1X+3967+distance+sensor) | Pi-compatible I2C breakout for distance readings; includes level shifting and supports 3–5 V logic. |
| 1 | [STEMMA QT / Qwiic JST-SH to four-wire jumper pigtail — Amazon search](https://www.amazon.com/s?k=STEMMA+QT+JST-SH+4+pin+female+jumper+cable) | Connects the Adafruit distance breakout to the Pi GPIO I2C pins without soldering. Verify pin order before power. |
| 1 | [Waveshare 5-inch HDMI LCD (C), 800×480, capacitive touch — Amazon search](https://www.amazon.com/s?k=Waveshare+5+inch+HDMI+LCD+C+800x480+capacitive+touch) | Display for the local web UI. Order the HDMI/touch version, not a DSI-only display. |
| 1 | [Waveshare-specified 5 V display supply and USB touch cable — Amazon search](https://www.amazon.com/s?k=Waveshare+5+inch+HDMI+LCD+5V+power+USB+touch+cable) | Powers the screen and carries touch input; do not assume the Pi can power it. Confirm the selected LCD revision's requirements. |
| 1 | [Audio-Technica ATR4697-USB boundary microphone — Amazon search](https://www.amazon.com/s?k=Audio-Technica+ATR4697-USB) | Standalone USB microphone for the display panel; 360-degree pickup, USB-C cable with USB-A adapter. Secure it away from direct fan airflow. It has no built-in physical mute; use the app's explicit microphone enable control and add a separate hardware mute/disconnect if physical mute is required. |
| 1 | [400-point solderless breadboard — Amazon search](https://www.amazon.com/s?k=400+point+solderless+breadboard) | Bench prototyping for the button/status indicator only; keep motor power off. |
| 1 | [Momentary normally-open breadboard pushbutton — Amazon search](https://www.amazon.com/s?k=momentary+normally+open+tactile+pushbutton+breadboard) | Local interaction/consent demo input. |
| 1 | [5 mm LED — Amazon search](https://www.amazon.com/s?k=5mm+LED+assortment) | Simple status indicator for Arduino simulation. |
| 1 | [330 Ω resistor, 1/4 W — Amazon search](https://www.amazon.com/s?k=330+ohm+1%2F4+W+resistor) | Current limiting for the status LED when driven by a 5 V Arduino output. |
| 1 set | [Male-to-male and male-to-female jumper wire kit — Amazon search](https://www.amazon.com/s?k=male+to+male+male+to+female+jumper+wires+breadboard) | For breadboard and sensor breakout connections; label both ends. |
| 1 | [Small plastic electronics project box — Amazon search](https://www.amazon.com/s?k=small+plastic+electronics+project+box) | Bench enclosure for the Pi/Arduino/sensor setup; not an overhead mounting part. |
| 1 | [HDMI cable, 0.5–1 m — Amazon search](https://www.amazon.com/s?k=short+HDMI+cable+1+meter) | Connects the Pi to the Waveshare display. |
| 1 | [USB-A extension cable, 0.5–1 m — Amazon search](https://www.amazon.com/s?k=USB+A+extension+cable+1+meter) | Optional; useful for placing the mmWave module away from the Pi and other electronics. |

Buy the sensor/software list as the first order. It supports a real presence
and distance readout, local display, and simulated Arduino channels. It does
not include a matched motor-and-ESC set or enable motor control.

The mmWave module's UART signal is not a USB port: the CP2104 is the USB-UART
adapter. Follow the exact LD2410C module documentation for its supply and
UART pinout; never connect its power pin to a Pi GPIO signal pin. For the
distance sensor, use the breakout's STEMMA QT connector and verify the cable
pin order before connecting it to the Pi's 3.3 V I2C pins.

### Motor-control parts for a later phase

| Qty | Part | Selection requirement |
|---:|---|---|
| 4 | Motors and propellers | Must be selected as a compatible set for the intended application; no specific model can be matched without motor/thrust requirements. |
| 4 | Manufacturer-compatible ESCs / motor drivers | Match the exact motor's voltage, current, and control interface; do not order until motor models are chosen. |
| 1 | Motor power supply and distribution | Must match all four motors/drivers and their rated simultaneous load; do not use the Pi supply. |
| 1 set | Motor-rated connectors, cables, and overcurrent protection | Match selected motor, driver, and supply specifications. |
| 1 | Physical motor-power disconnect / emergency stop | Select to suit the motor power system; independent of the Pi, browser, and Arduino software. |

Propellers, containment, cross-frame, suspension, display-panel mounting, and
ceiling hardware belong to the separate mechanical design. Keep all propellers
and motor power disconnected while building the sensor/software prototype.
Do not order a particular driver or motor power supply until the exact motor
models are known.

### Software components

| Component | Prototype implementation |
|---|---|
| Raspberry Pi OS | Local operating system; application works without cloud access. |
| Web UI | Existing `index.html`, served locally; retain simulation mode for development. |
| Pi application/API | Python 3 service (for example, FastAPI) reads sensors, tracks reading freshness/health, runs interaction state logic, and serves local status/control endpoints. |
| Arduino firmware | Arduino IDE with C/C++. Reads validated serial commands, simulates four motor-channel states, and reports status/faults. Keep real ESC outputs disabled in this prototype. |
| Pi-to-Arduino link | USB serial with a documented message format, such as newline-delimited JSON; reject malformed, stale, or unsupported commands. |
| Presence-sensor reader | Python serial/USB reader using the selected mmWave module's documented protocol. |
| Distance-sensor reader | Python I2C reader using the selected breakout manufacturer's library and pinout. |
| Microphone input | Browser Web Audio API or a local Python audio library, enabled only after explicit user action; retain physical mute and a visible listening indicator. |
| Tests and logs | Python unit tests for state transitions and sensor faults; timestamped local logs for sensor health, simulated commands, and Arduino responses. |
| Dependencies | Python virtual environment with pinned packages; record Arduino library names and versions. Host required assets locally for offline operation. |

### Detection and control architecture

Use mmWave for room presence and the range sensor for distance. The Pi service
combines readings, tracks sensor health, and provides status to the UI. The
browser communicates with the Pi service, never directly with Arduino pins or
motor drivers. The Pi sends constrained commands to the Arduino over USB
serial; the Arduino reports its state. Keep outputs disabled while testing
this chain.

Treat missing, stale, or conflicting readings and loss of Pi-to-Arduino
communication as unknown/fault states. Test them in software while motor
outputs remain disabled. Presence and distance sensors support interaction;
they are not safety devices.

### Suspension and motion requirements

The sphere, internal cross, propeller containment, ceiling suspension,
secondary retention, dynamic load rating, and display-panel support must be
designed together and approved for the building. A hanging assembly can behave
as a pendulum; changing propeller thrust can introduce unpredictable forces
and oscillation. The proposed diagonal motor grouping does not by itself
guarantee forward/back motion. No motorized overhead test or installation is
appropriate until an engineer has analyzed the forces, failure modes, stops,
clearances, and containment and approved a test plan.

## Step-by-step electronics and software setup

This sequence installs and tests the electronics/software prototype on a
bench. The motor/ESC outputs remain disabled throughout.

### 1. Prepare the Pi and display

1. On a computer, use Raspberry Pi Imager to write Raspberry Pi OS to the
   microSD card. Insert it into the Pi and complete first-boot setup.
2. Connect the touchscreen to the Pi's HDMI output and power the display using
   the display maker's specified cable/supply. Connect the Pi to its own
   USB-C supply.
3. Connect the Pi to the local network for setup and updates. The application
   itself should run locally and not require cloud access.
4. On the Pi, update Raspberry Pi OS and install Python 3, `python3-venv`,
   `python3-pip`, and `git` using the OS package manager. Create a project
   directory and Python virtual environment for the Aera service.
5. The web page exists, but the Pi sensor/API service and Arduino firmware
   still need to be created. Once present, install their pinned Python and
   Arduino-library dependencies from the project's dependency files.

### 2. Install and test the people sensors

1. Shut down and unplug the Pi before attaching sensor leads.
2. Connect the mmWave module using that exact board's manufacturer pinout,
   supply-voltage requirements, and documented UART/USB interface. Do not
   infer pin voltage from the module name or sensor chip.
3. Connect the time-of-flight breakout using its documentation. If using I2C,
   enable I2C in Raspberry Pi OS and use only a breakout documented as
   compatible with the Pi's 3.3 V logic. Never apply 5 V to a Pi GPIO pin.
4. Secure the sensor boards on the bench so they cannot short against metal.
   Reconnect Pi power and verify each sensor reader reports valid, timestamped
   data before mounting sensors in a panel.
5. Test presence by entering, leaving, and sitting still in the sensors'
   fields of view. Test distance at several known positions. Record range,
   coverage, missed detections, and false detections; these sensors are not
   safety devices.
6. Disconnect each sensor separately. The application should mark data as
   unavailable/stale and display a fault rather than assuming a person is
   absent or assigning a default distance.

### 3. Set up the Pi application and web UI

1. Serve the existing `index.html` from the Pi and confirm the page opens on
   the touchscreen. Keep its sensor controls in simulation mode at first.
2. Implement the Pi-side local API and sensor service. It should report
   presence, distance, timestamp/freshness, sensor faults, microphone state,
   Arduino connection, and requested (simulated) motor group.
3. Add the state logic for present/absent, distance/approach, sensor fault,
   explicit microphone consent, and simulated motor requests. No network
   connection should be needed to request the off state.
4. Add unit tests for normal readings and for absent, stale, conflicting, or
   disconnected sensors before connecting real sensors to motor behavior.

### 4. Set up the Arduino serial prototype

1. Install Arduino IDE on a computer and select the exact Arduino board. Use
   the USB data cable to connect it to the Pi.
2. Create and upload firmware that starts in a disabled/simulation state,
   reads a documented serial message format, rejects unknown commands, and
   reports status/faults. Keep real ESC outputs disabled.
3. Represent north, west, south, and east as four software channels. Add
   north+west and south+east as named simulated groups; do not connect ESC
   signal inputs or motor power for this test.
4. Send test messages from the Pi and check that the Arduino reports the
   expected simulated channels. Verify reset, malformed messages, and loss of
   USB serial leave the simulated output off.

### 5. Run the end-to-end software tests

1. Start the Pi sensor service and web UI, connect the presence and distance
   sensors, and confirm their values/status appear on the screen.
2. Test entering, leaving, remaining still, approaching, and sensor
   disconnection. Verify the UI updates and indicates any sensor fault.
3. Connect the Arduino over USB and test simulated channel/group requests.
   Unplug the USB cable and restart the Pi; the interface should report loss
   of connection and request off.
4. Test the microphone only after deliberate user consent. Verify the
   physical mute and visible listening status.
5. Keep propellers, powered ESCs, and motor supply disconnected during these
   steps. The existing project does not yet include the Pi service or Arduino
   firmware, so those software components must be implemented before this
   procedure can be completed.

## Control software and fail-safe requirements

Use the Raspberry Pi as the local UI and sensor-service host and the Arduino
as the motor-command processor. Keep occupancy and distance logic local;
cloud access must never be required to request an off state. Keep motor
outputs simulated and disabled in this prototype.
Sensor absence, unknown readings, stale data, disagreement, reboot,
communication/controller fault, or emergency stop must disable motor
commands. A separate physical motor-power disconnect remains independent of
the application.

The repository's `index.html` is still a browser-only prototype. It simulates
sensor conditions with buttons and does not read actual people sensors, run
Arduino firmware, or control motor drivers. Automatic detection requires a
Pi sensor service and compatible, tested sensors. Real motor control requires
the approved motor/driver system and independent safety hardware described
above. The page also loads p5.js and an embedded sketch from the internet; an
offline installation needs a locally hosted sketch and library.
