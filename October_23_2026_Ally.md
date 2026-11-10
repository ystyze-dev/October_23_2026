# Aera: an embodied ally

The fan is an expressive companion: it notices a person, listens only when
invited, and responds with poetry. The browser page is a visual prototype; it
does not control an appliance or detect people automatically.

## Project definition

The chosen appliance is **not the NORTHSKY wall/workstation fan**. Select one
forward-facing fan whose manufacturer explicitly approves the exact overhead
orientation, cage, and suspension arrangement. The fan axis should point
horizontally into the room; the blades should not blow down like a conventional
ceiling fan. The protective cage contains the fan. A separate, engineered
hanger/pivot and suitable actuator swing the contained fan assembly forward
and back. Blade count does not make a fan swing.

The display, microphone, motion/distance sensors, and controller live in a
panel below the fan, with attachment and load support approved separately.
Choose a fan/controller with a documented, isolated low-voltage speed/control
interface compatible with a Pi interface (for example, manufacturer-documented
Modbus RTU/RS-485 or 0–10 V input). A Pi GPIO must never connect directly to
fan motor or mains wiring. A listed fan with only a pull chain or IR remote
does not meet the automatic-control requirement unless its manufacturer
documents an approved interface.

## Intended interaction

1. If no person is in the room, fan motor and swing actuator are off. If the
   person is closer than 2 ft (0.61 m), both stop as a project behavior; this
   hobby sensor is not a certified personnel-protection device.
2. At 2–8 ft, motion detection makes the fan attentive with blades stopped.
   Motion classified as approach toward the fan turns on the fan. Motion
   direction must be determined from successive distance readings, not a
   single motion event.
3. If the person is in the room but farther than 8 ft, keep the fan attentive
   but stop the blades and display a mixed English/Spanish poem. A deliberate
   blow at the panel microphone dissipates the poem and restarts airflow,
   unless the person is absent, under 2 ft, or a sensor/controller fault exists.
4. Laughter requests fast staccato fan speed. Sustained laughter for more than
   2 minutes additionally starts the rated swing mechanism. Empty-room and
   under-2-ft off conditions take precedence. Audio loudness alone is not
   reliable laughter recognition; production control needs validated
   processing, an explicit button interaction, or another reviewed input.
5. Any uncertain presence/distance reading, sensor failure, reboot, or
   controller fault must fail off. Preserve the fan guard and an independent
   physical stop/disconnect.
6. Swing is an independent motion of the approved caged assembly, not the
   number or rotation of fan blades. Its pivot, restraint, actuator, travel
   stops, and control must be rated for the assembly by the responsible
   mechanical installer. Do not implement swing by dangling a running fan on
   an improvised cable.

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
| 1 | Forward-facing directional fan with protective cage | Select a model whose maker permits the intended horizontal-axis suspended use, cage, and required orientation; verify airflow for the 3.5 m room and check its motor/control documentation. Candidate research is pending. |
| 1 | Raspberry Pi 5, 4 GB | Runs the local web UI, p5.js visualization, poem selection, and sensor application. No mains switching. |
| 1 | Raspberry Pi 27 W USB-C Power Supply, region-matched | Dedicated 5.1 V / 5 A USB-C Pi supply. Do not power the fan from it. |
| 1 | 64 GB high-endurance microSD card and compatible reader | Pi OS and local poems; the reader is needed to prepare the card from a computer if one is not built in. |
| 1 | Raspberry Pi case with Pi 5 cooling (case fan/heatsink) | Protects and cools the controller. Keep it in the low-voltage panel, clear of the appliance fan motor and airflow obstructions. |
| 1 | 5-inch HDMI touchscreen with documented 5 V USB power input | Mount in the separate display/control panel below the fan; confirm panel dimensions and current draw. Include its maker-specified certified 5 V supply if required; do not assume the Pi can power the display over USB. |
| 1 | USB digital microphone with physical mute switch | USB-powered (typically 5 V); verify the selected model. Mount in the panel, not at the fan grille. Add a foam windscreen; leave muted until the person opts in. |
| 1 | VL53L1X time-of-flight breakout with 3.3 V I2C logic | Power and I2C pin levels vary by breakout; use only the selected board's documented input voltage and 3.3 V-compatible logic. Measures a person in its field of view around the 2 ft and 8 ft thresholds. This is a prototype input, not a safety interlock. |
| 1 | 24 GHz mmWave presence module with documented UART/logic levels (for example, an HLK-LD2410C module) | Occupancy sensing when a person is relatively still. Confirm the exact board's supply voltage and UART logic levels from its datasheet before connecting. Verify room coverage, privacy behavior, false negatives, and motion through walls. |
| 1 | Momentary low-voltage interaction button | Gives a local opt-in/stop control. Choose a switch and enclosure suitable for the controller's 3.3 V logic. |
| 1 | Low-voltage status LED or small indicator module | Indicates microphone active and controller fault/status; use a current-limiting part/module rated for the chosen logic voltage. |
| 1 | Optional Arduino Nano ESP32 | Not required if the Pi reads the low-voltage sensors directly. If used, power from its documented USB-C input (5 V); keep its GPIO at 3.3 V logic. It must not switch or connect to the fan's mains circuit. |
| 1 each | Short HDMI cable; display's specified USB power cable; USB microphone cable; Pi USB-C power cable; documented sensor pigtails | Choose lengths to suit the panel and keep low-voltage cables clear of the fan and its suspension. Secure strain relief in the panel. |
| 1, if needed | Powered USB hub with its own listed low-voltage adapter | Only if the selected screen/microphone exceed available Pi ports or power budget. Verify the manufacturer requirements; do not assume power can be daisy-chained. |
| as needed | Low-voltage enclosure, cable glands/strain relief, standoffs, labels, and panel fasteners | Enclose and support display electronics. Choose panel attachment hardware for its independently approved support; do not hang it from the fan unless the manufacturer and installer approve the added load. |
| 1 | Manufacturer-approved suspended mounting/cage kit for the selected fan | Exact model, orientation, structural support, and permitted loads must come from the fan maker and responsible installer. |
| 1 | Rated pivot/swing actuator and motion controller | Separate engineered system sized and approved for the full guarded fan assembly, with travel limits and safe stop behavior. Do not choose an actuator based on blade count alone. |
| 1 | Fan maker's documented controller/interface plus isolated Pi interface | Select only after the actual fan is chosen. Example interface families: isolated USB-to-RS-485 for documented Modbus RTU, or a properly isolated 0–10 V interface. Pi GPIO alone is neither. |

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

### Suspension and swinging materials

The actual suspension/cage kit, load-rated bracket, structural attachment,
restraints, and allowable travel must be specified for the chosen fan and
building by the manufacturer and responsible installer. The decorative
canopy in the concept is not a structural part. The display panel also needs
its own approved support. The swing actuator must move the whole guarded
assembly through controlled, limited travel; cable alone does not provide
controlled swinging. These structural and motorized parts cannot be selected
until a specific fan and mechanical design are approved.

## Installation sequence

### 1. Gather and verify the components

1. Select the forward-facing fan first. Record its exact model, nameplate,
   manual, horizontal-axis mounting approval, guarded/suspended mounting kit,
   airflow data, and documented speed-control interface.
2. Make a parts checklist from the table. Confirm the screen's connector and
   power needs, sensor breakout-board pinouts/logic levels, microphone USB
   connection, and Pi supply before connecting anything.
3. Have the responsible installer approve the suspension cage, structural
   support, swing pivot/actuator, travel stops, assembly weight, and panel
   support. Confirm the building structure, clearances, electrical supply,
   and local requirements.
4. Confirm the fan maker's controller protocol, required isolation, and
   compatible Pi interface before ordering control electronics.
5. Keep the fan disconnected during all student electronics assembly. Do not
   open the fan or alter its motor, power cord, or wiring.

### 2. Bench-build the low-voltage controller

1. Use Raspberry Pi Imager on a computer to install Raspberry Pi OS on the
   microSD card. Eject the card safely and insert it into the Pi.
2. Place the Pi in its ventilated case and fit the case maker's cooling.
   Keep the Pi and its power supply outside the fan enclosure and away from
   hot or moving parts.
3. With power disconnected, connect the touchscreen to HDMI and its
   manufacturer-specified USB power input. Connect the microphone to USB but
   leave its physical mute switch on.
4. Connect the VL53L1X breakout to the Pi I2C bus using that exact board's
   documentation: Pi 3.3 V, ground, SDA, and SCL. Standard Pi I2C1 physical
   header pins are 1 (3V3), 6 (ground), 3 (GPIO2/SDA), and 5 (GPIO3/SCL).
   Verify connector orientation and breakout input voltage first. Never put
   5 V on a Pi GPIO pin.
5. Connect an optional mmWave sensor only after verifying its supply and
   UART logic levels. Use its documented pinout; do not assume modules with
   the same sensor chip have identical interface voltages.
6. Connect the low-voltage interaction button and status indicator according
   to Raspberry Pi GPIO documentation. Label both ends of each sensor wire
   with the project color convention above. Inspect for shorts, then power
   the Pi using its specified USB-C supply.

### 3. Install and test software and sensors on the bench

1. Boot the Pi and confirm the screen and local web page work. Host p5.js and
   the poem data locally if the project must work offline.
2. Enable I2C in Raspberry Pi OS and run a library/example for the selected
   VL53L1X board. Check readings over the approach path, especially near
   0.61 m (2 ft).
3. If used, configure the mmWave module using its manufacturer documentation
   and confirm room-presence events arrive on the documented interface.
4. Test the interaction state machine with fan control physically absent:
   empty room, unknown distance, sensor fault, or distance under 2 ft must
   show off; 2–8 ft motion shows attentive; approach turns on airflow; over
   8 ft waits for a blow; laughter shows staccato and
   sustained laughter over 2 minutes requests swing; the simulated blow
   control dissipates the poem and resumes airflow.
5. Test occupancy with a person sitting still, leaving, and entering. Test
   stale/disconnected sensors and reboot; uncertain inputs should display a
   fault/off state, not start the fan.
6. Test the microphone only by intentionally enabling it. Check physical
   mute, listening indicator, opt-in permission, and manual stop. Loudness
   detection is a rough blow cue and can mistake other sounds for breath.

### 4. Assemble the display/control panel

1. Lay out the touchscreen, microphone grille/windscreen, mute/interaction
   button, status LED, and sensor apertures on a panel mock-up below the fan.
2. Check that the panel does not block the fan intake/outlet, adjustment
   access, or service access. Keep the microphone away from direct airflow to
   reduce wind noise.
3. Mount the components in a ventilated low-voltage enclosure with standoffs.
   Add strain relief for HDMI, USB, power, and sensor cables; label them at
   both ends.
4. Confirm how the panel will be independently supported. Do not hang it
   from the fan, fan grille, or decorative canopy unless its manufacturer
   and the responsible installer approve that exact added load.
5. Repeat the bench tests with the assembled panel before it goes near the
   suspended fan.

### 5. Have the overhead fan and final installation commissioned

1. Give the school's written approval, fan manual/nameplate, proposed
   assembly weight, and mounting-kit documentation to the responsible
   installer. Confirm approved orientation, support, clearances, and panel
   attachment before overhead work.
2. The responsible qualified installer performs overhead structural mounting
   and any 120 V AC work using the approved fan kit, manufacturer instructions,
   and local requirements. This guide intentionally does not give ceiling
   fastening or mains conductor/wiring instructions.
3. Have the installer verify the fan is secure, guarded, properly powered,
   serviceable, and has an independent physical disconnect before anyone
   operates it. Keep students clear during installation and commissioning.
4. Place the display panel only on its approved independent support below the
   fan. Route its low-voltage cables as approved, separated from mains parts,
   moving parts, and hot surfaces.
5. If the selected fan's manufacturer and installer approve a specific
   listed electronic controller, the electrician installs it. Only then may
   software interface with its documented isolated low-voltage input.
   Without such a controller, operate the fan manually and use the Pi for
   sensor/display/poetry only.
6. Commission with the installer: verify manual stop/disconnect first, then
   sensor fault/off behavior, under-2-ft off, occupancy states, and display
   responses. Do not use prototype sensors as a blade-contact safety system.

## Control software and fail-safe requirements

Use the Raspberry Pi as the UI/application host. A separate Arduino is
optional and is not a mains controller. Keep presence/distance logic local;
do not depend on cloud access for any off request. Implement a state machine
with safety precedence: absent, under 2 ft, unknown distance, sensor fault, or
controller fault means fan and swing actuator off; at 2–8 ft, motion means
attentive/stopped while approach enables airflow; beyond 8 ft, stop blades
until a blow; a blow clears the poem and resumes airflow unless an off
condition applies. Laughter gives staccato airflow in the permitted range;
continuous laughter beyond 2 minutes requests the independently-rated swing
mechanism, subject to all off conditions. Log sensor status, provide a
physical stop, and require explicit microphone consent.

The repository's `index.html` remains a browser-only prototype. It simulates
sensor conditions with buttons; it has no Pi GPIO, Arduino firmware, sensor
driver, authenticated hardware API, fan product selection, swing actuator, or
appliance control. The real fan, guard, suspended mounting, actuator, and
controller still need to be identified and approved for the full assembly and
building before installation.
The current page also loads p5.js and an embedded sketch from the internet; an
offline installation would need a locally hosted sketch and library.
