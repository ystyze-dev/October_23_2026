# Aera: an embodied ally

The fan is an expressive companion: it notices a person, listens only when
invited, and responds with poetry. The browser page is a visual prototype; it
does not control an appliance or detect people automatically.

## Important NORTHSKY compatibility and mounting limits

The user-provided product details identify the NORTHSKY NSS-WM12 / retail SKU
20198251875 as a 120 V AC, 60 Hz, UL-listed, three-speed wall/workstation fan
with a manual pull chain and tilt bracket. The reported airflow and motor
ratings vary by product listing; confirm them against the exact fan's label
and manual before purchase or installation.

This is a **wall-mount fan**, not a ceiling fixture. UL listing alone does not
approve overhead suspension. Do not hang it from the ceiling, attach a display
or microphone to its grille or housing, or let it swing. Use it only with its
manufacturer-approved wall bracket and mounting method unless NORTHSKY
provides written approval and a specified overhead kit for this exact model.
For a lighting-fixture appearance, choose a fan explicitly listed and
manufacturer-approved for ceiling suspension instead.

The pull-chain motor has no confirmed electronic control input. Neither a
Raspberry Pi nor an Arduino may be connected directly to the 120 V fan, pull
chain, or motor. Do not use a hobby relay, triac, MOSFET, or PWM output to
switch or vary this appliance. Its speed, laugh response, and power state
cannot be automated until NORTHSKY confirms a compatible, appropriately
listed motor controller and a qualified electrician approves the installation.
The prototype below therefore controls only its own display and low-voltage
sensors; fan states are visual simulations.

## Intended interaction

1. A room-presence sensor detects an occupant and a separate distance sensor
   estimates distance. At about 2 ft (0.61 m), the fan may enter an attentive
   state. If measured distance is under 2 ft, command **off** as a conservative
   design rule. No single hobby sensor is a certified personnel-protection
   device; retain the fan grille, required clearances, and independent physical
   power disconnection.
2. With the person absent, the requested state is off. Room occupancy must be
   inferred from sensors with suitable room coverage and a conservative
   configurable absence timeout; one motion sensor can miss a still person.
3. A muted-by-default microphone in the display panel listens only after an
   explicit interaction. A breath-like sound makes the p5.js poem dissipate.
   Basic microphone loudness detection is not reliable breath, speech, or
   laughter recognition; use a deliberate blow interaction or validated
   signal processing and show a listening indicator.
4. A recognized laugh can request a fast, staccato response only if the
   distance and occupancy rules allow it and a manufacturer-approved fan
   controller exists. The fan's manual three-speed pull chain alone cannot
   provide this behavior.
5. After two continuous hours of confirmed room occupancy without a detected
   blow, show an irritated poem/animation. Reset this timer after a detected
   blow or confirmed room-empty state. Do not physically swing this wall fan.
   Its tilt bracket is manual; a swinging suspended appliance is not a safe
   substitute for a visual animation.
6. A displayed poem dissipates when breath is detected, using the p5.js
   particle effect. The app should show when sensing is enabled and provide a
   manual clear/stop option.

Presence, distance, breath, and laughter inputs are not safety-rated. Sensor
failure, loss of power, reboot, unknown distance, and disagreement between
sensors must never cause a fan start. The intended fail-safe state is fan
power off; the controller cannot implement that until a compatible certified
control interface is established.

## Suggested prototype hardware

These are low-voltage prototype parts, not a finished fan-control system.
Confirm connector, power, and mounting details against the exact part
datasheets before ordering.

| Qty | Part | Purpose and requirements |
|---:|---|---|
| 1 | Raspberry Pi 5, 4 GB | Runs the local web UI, p5.js visualization, poem selection, and sensor application. No mains switching. |
| 1 | Raspberry Pi 27 W USB-C Power Supply, region-matched | Dedicated 5.1 V / 5 A USB-C Pi supply. Do not power the fan from it. |
| 1 | 64 GB high-endurance microSD card | Pi OS and local poems. |
| 1 | 5-inch HDMI touchscreen with documented 5 V USB power input | Mount in the separate display/control panel below the fan; confirm panel dimensions and current draw. Use the display maker's certified 5 V supply if required; do not assume the Pi can power the display over USB. |
| 1 | USB digital microphone with physical mute switch | USB-powered (typically 5 V); verify the selected model. Mount in the panel, not at the fan grille. Add a foam windscreen; leave muted until the person opts in. |
| 1 | VL53L1X time-of-flight breakout with 3.3 V I2C logic | Power and I2C pin levels vary by breakout; use only the selected board's documented input voltage and 3.3 V-compatible logic. Measures a person in its field of view near the 2 ft threshold. This is a prototype proximity input, not a safety interlock. |
| 1 | 24 GHz mmWave presence module with documented UART/logic levels (for example, an HLK-LD2410C module) | Occupancy sensing when a person is relatively still. Confirm the exact board's supply voltage and UART logic levels from its datasheet before connecting. Verify room coverage, privacy behavior, false negatives, and motion through walls. |
| 1 | Momentary low-voltage interaction button | Gives a local opt-in/stop control. Choose a switch and enclosure suitable for the controller's 3.3 V logic. |
| 1 | Low-voltage status LED or small indicator module | Indicates microphone active and controller fault/status; use a current-limiting part/module rated for the chosen logic voltage. |
| 1 | Optional Arduino Nano ESP32 | Not required if the Pi reads the low-voltage sensors directly. If used, power from its documented USB-C input (5 V); keep its GPIO at 3.3 V logic. It must not switch or connect to the fan's mains circuit. |
| 1 each | Short HDMI cable, USB-A-to-USB cable for display (if specified), USB microphone cable, USB-C Pi power cable, sensor pigtails | Use the connectors and lengths specified by the selected display/sensor. Secure strain relief in the panel. |
| as needed | Low-voltage rated enclosure, cable glands/strain relief, standoffs, fasteners, labels | Enclose and support the display electronics and separate low-voltage wiring. Select enclosure and mounting hardware for the actual panel and location. |
| 1 | NORTHSKY-approved wall bracket and manufacturer-specified fasteners | Use only as directed in the fan manual and for the actual wall construction. The panel must have its own independently supported mount unless NORTHSKY explicitly approves attaching its load to the fan bracket. |
| 1 | Manufacturer-approved, listed fan controller (only if NORTHSKY confirms compatibility in writing) | Required before any automatic on/off or speed control. Exact model and interface remain unknown; do not substitute a generic relay or dimmer. Have the control and mains installation handled by a qualified electrician. |

### Low-voltage wiring and color convention

Prefer USB for the display, microphone, and controller. For sensor pigtails,
use the sensor manufacturer's cable and pinout. A project may choose **red for
3.3 V**, **black for low-voltage ground**, **yellow for I2C SCL**, **blue for
I2C SDA**, and **green/white for UART TX/RX**, but these colors are only a
project labeling convention, not a universal electrical standard. Label both
ends and verify every conductor against the exact board pinout before applying
power. Raspberry Pi GPIO is 3.3 V logic and is not 5 V tolerant. Keep all
prototype wiring low voltage and physically separate from the appliance's
120 V wiring. Do not specify or repurpose mains conductor colors: an
electrician must follow local code, the appliance instructions, and the
approved controller documentation.

### Materials if a ceiling-fixture appearance is required

There is **no safe hanging-material list for the NSS-WM12**: it is described as
a wall-mount fan, and no overhead kit or approval has been provided. Do not
hang it from a hook, cord, chain, lighting canopy, or improvised bracket. For a
replacement fan explicitly approved for ceiling installation, have the
manufacturer and installer specify the compatible factory mounting kit,
structural support/fan-rated box if required by that model and building,
manufacturer-specified fasteners, canopy, and any required secondary restraint.
The display panel needs its own support rated for its weight; do not suspend
it from the fan or attach it to the fan's grille. Exact support materials
depend on the replacement product, ceiling construction, and local code.

## Installation sequence

### 1. Confirm a safe appliance/mounting combination

1. Photograph the fan nameplate and find the exact NSS-WM12 manual.
2. Ask NORTHSKY whether this exact wall-mount model is approved for the
   proposed location, what bracket and fasteners it requires, and whether a
   listed controller supports automatic power and speed changes. Obtain
   written confirmation and the controller part number.
3. Unless NORTHSKY expressly approves overhead mounting and provides its
   specified kit, stop the ceiling-fixture plan. Install the fan only on the
   approved wall bracket. For a ceiling-hung look, substitute a
   ceiling-rated fan/fixture.
4. Have a qualified installer verify the building structure, fan location,
   electrical supply, clearances, and local permit/code requirements. Do not
   treat a sensor cutoff as protection from contact with fan blades.

### 2. Assemble the separate low-voltage display panel

1. Select the touchscreen, microphone, button, status indicator, and enclosure
   so the panel can be positioned below the fan without blocking its intake,
   airflow, adjustment, or service access.
2. Mount the display, microphone with windscreen, mute/interaction controls,
   and sensor windows to the panel. Do not attach the panel to the fan unless
   both manufacturers approve the added load and attachment.
3. Fit strain reliefs and standoffs. Route and label only the low-voltage
   cables; keep them separate from any mains compartment and away from moving
   or hot parts.
4. Power the Pi and display from their manufacturer-specified, certified
   low-voltage supplies. Do not connect any prototype board to the fan's
   power, chain, or motor.

### 3. Install and test the sensors

1. With the fan disconnected from the work area during prototype testing,
   connect the VL53L1X and optional mmWave module to the controller using
   their documented power, logic, and connector requirements.
2. Aim and calibrate the distance sensor for the intended approach path.
   Check its error and blind spots around 0.61 m (2 ft); configure uncertain
   or missing distance as off.
3. Test room-occupancy coverage with a person sitting still, leaving, and
   entering. Configure an absence timeout that fails to off and cannot be
   defeated by a stale reading.
4. Test button, microphone mute, and visible listening indicator. Request
   microphone permission only after a deliberate opt-in.
5. Run the display and sensor demo with fan control physically absent.
   Confirm unknown distance, under-2-ft distance, sensor loss, and no
   occupancy all produce the off indication. Confirm laugh and two-hour
   irritation states cannot override those conditions.

### 4. Commission any real fan control

1. Do not proceed unless NORTHSKY has confirmed a specific listed controller
   for this fan and the approved wall-mounted installation.
2. Have a qualified electrician install and test the fan and controller
   according to the manufacturer's instructions and local electrical code.
   This guide intentionally does not give mains wiring or conductor-color
   instructions.
3. Have the electrician verify manual operation, controller compatibility,
   independent disconnect, and the final safe mounting/clearance.
4. Only after that approval, integrate the controller through its documented
   low-voltage interface, if any. Test off behavior and sensor-fault behavior
   first; never let a Raspberry Pi/Arduino GPIO connect to mains.
5. If there is no approved control interface, retain the fan's manual control
   and use the electronics for display/poetry only. Do not claim automatic
   fan control.

## Control software and fail-safe requirements

Use the Raspberry Pi as the UI/application host. A separate Arduino is
optional and is not a mains controller. Keep presence/distance logic local;
do not depend on cloud access for a safety-related off request. Implement a
state machine with safety precedence: absent, under 2 ft, unknown distance,
sensor fault, or controller fault means fan off; otherwise allow only
manufacturer-approved commands. Log sensor status, provide a physical stop,
and require explicit microphone consent. A two-hour no-blow interval may
change the display to an irritated poem but must not defeat the off rules or
move the physical fan.

The repository's `index.html` remains a browser-only prototype. It simulates
sensor conditions with buttons; it has no Pi GPIO, Arduino firmware, sensor
driver, authenticated hardware API, or appliance control. The model-specific
controller and ceiling-hanging materials cannot be finalized until NORTHSKY
approves the installation/control and the exact installation location and
local requirements are known.
The current page also loads p5.js and an embedded sketch from the internet; an
offline installation would need a locally hosted sketch and library.
