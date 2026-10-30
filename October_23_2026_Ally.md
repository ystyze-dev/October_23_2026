This is my Ally manifesto

Welcome
My AI should pretend to be a hesitant fan.

## Hardware for the fan

The fan is an embodied ally: it notices a person, listens when invited, and responds gently. A small Raspberry Pi can coordinate the sensors, display, poetry, and fan control.

### Components

1. **Controller: Raspberry Pi 5 (4 GB)**
   Runs the interaction program, connects the components, and mediates between a person's presence or speech and the fan. Speech recognition or an AI service can run locally or remotely; the interaction should still have a simple fallback if AI is unavailable.
2. **Distance sensor: VL53L1X time-of-flight sensor**
   Detects a person approaching and estimates distance. It connects to the Pi over I2C. Use its reading to wake the display or invite interaction, not as the only way to control the fan.
3. **Microphone: USB microphone with a physical mute switch**
   Captures speech for an explicitly invited interaction. A visible listening indicator and a clear mute control help make listening understandable and optional. The microphone connects to the Raspberry Pi; the AI can also communicate with your phone through the phone connection below.
4. **Display: 5-inch HDMI touchscreen**
   Shows the current state, interaction prompts, and selected poetry. A non-touch display can be used if interaction is handled by voice or a separate button.
5. **Poetry data source: microSD card or USB storage**
   Stores a curated collection of poems as plain text or JSON. The program can select a poem in response to an interaction and display it without needing an internet connection.
6. **Fan and driver: 12 V DC fan, logic-level MOSFET driver module, and separate 12 V power supply**
   The Pi controls the driver with a GPIO/PWM signal; the driver switches power to the fan. Do not connect a fan directly to a Pi GPIO pin. Keep the fan on its own properly rated supply, share ground between the Pi and driver where the driver requires it, and follow the driver and fan wiring diagrams. Avoid mains-voltage fan control for this prototype.
7. **Optional interaction controls: momentary button and status LED**
   A button lets a person start or stop an interaction, while an LED indicates when the microphone is active or the fan is running.
8. **Phone connection: your existing iPhone or Android smartphone and the Pi's built-in Wi-Fi**
   Make the phone interface a responsive web app that works in iPhone Safari and Android Chrome, so a dedicated app or extra phone hardware is not required. It should let you chat with the AI, read the poetry, see fan status, and send fan commands. You can optionally add the web app to the phone's home screen. Connect the phone and Pi to the same trusted Wi-Fi network; the Pi 5 has built-in Wi-Fi. If using the phone's microphone in the web app, serve it over HTTPS and ask for microphone permission only after the user taps to speak—mobile browsers generally block microphone access on ordinary HTTP pages. Provide text chat as a fallback. Protect access with sign-in and do not expose the Pi directly to the public internet. If there is no Wi-Fi router, the Pi can instead be configured as a password-protected hotspot. Internet access is needed for a cloud AI service; a local AI can work without it.

### Interaction flow

1. The distance sensor notices someone nearby and the display offers a quiet invitation.
2. The microphone is used only when the person starts or accepts an interaction; the AI interprets the speech and decides whether to respond with the fan, a message, or a poem. From an iPhone or Android phone on the same Wi-Fi, the person can also use the web app to chat, read the response, and control the fan. Phone voice input requires HTTPS and permission; typing remains available as a fallback.
3. The program selects poetry from local storage and displays it. The fan controller uses bounded speed settings and a timeout, so a sensor or software error cannot leave the fan running indefinitely.

For a first prototype, test the display, sensor, microphone, and poem selection before connecting the fan. Then test fan control with the fan's low-voltage supply disconnected from the Pi's power circuit, and verify the driver wiring and current rating before operation.
