# 🌆 Adaptive Smart Lighting System

![Platform] - TinkerCad
# Link to project: 
https://www.tinkercad.com/things/ih4shZ0EZwz-arduinofinalprojectmahammadmammadov?sharecode=nmJ4aO-vhTacv-uboDY9LKLDvIw1i3OSUhfNhR19Xc4

# IF ANY PROBLEMS WITH THE LINK AND PROJECT REVIEWING PLEASE CONTACT ME:

Contact: mahammadmammadov0091@gmail.com

An autonomous, adaptive outdoor lighting mechanism engineered on the Arduino open-source hardware architecture. This project uses an analog Light-Dependent Resistor (LDR) and Pulse-Width Modulation (PWM) to create a dynamic "sunset effect," smoothly transitioning light intensity based on real-time environmental ambient light levels.

---

## 📖 Project Overview

Traditional municipal streetlights rely on discrete binary (ON/OFF) mechanical relays or crude timers. This archaic approach causes massive high-current inrush spikes on electrical grids and fails to adapt to transitional weather (e.g., sudden overcast clouds).

This project shifts the paradigm to **continuous analog data translation**. By capturing ambient light levels via a photo-resistive voltage divider and mapping them inversely to an LED (or Solid-State Relay) via PWM, the system dynamically dims or brightens the light. 

**Key Benefits:**
* Eliminates sudden load spikes on local electrical grids.
* Enhances energy conservation by only using the required amount of power.
* Automatically adjusts to localized weather anomalies.

---

## 🛠️ Hardware Requirements

| Component | Specification / Pin | Purpose |
| :--- | :--- | :--- |
| **Arduino Uno R3** | Atmega328P Core | Central processing, ADC reading, and PWM generation. |
| **Photoresistor (LDR)** | Analog Pin `A0` | Cadmium Sulfide (CdS) sensor for environmental telemetry. |
| **Pull-Down Resistor**| `4.7kΩ` | Forms a voltage divider with the LDR for analog readings. |
| **LED (Solid-State Emitter)**| Digital Pin `9` (PWM) | Simulates the municipal street lamp. |
| **Current-Limiting Resistor**| `220Ω` | Protects the LED from thermal runaway. |
| **Breadboard & Wires** | Standard | For circuit prototyping. |

> **Note on Resistor Selection:** A `4.7kΩ` resistor is explicitly chosen over a generic `10kΩ` variant to maximize the analog voltage sweep across typical urban twilight lux ranges, ensuring higher sensitivity during dusk.

---

## 🔌 Circuit Wiring

1. **Power:** Connect Arduino `5V` to the breadboard positive rail, and `GND` to the negative rail.
2. **Sensor Loop (Voltage Divider):**
   * Connect one leg of the **LDR** to `5V`.
   * Connect the second leg of the **LDR** to Arduino Analog Pin `A0`.
   * Connect the `4.7kΩ` resistor from that same second LDR leg to `GND`.
3. **Output Loop:**
   * Connect Arduino Digital Pin `9` (PWM) to the Anode (long leg) of the **LED**.
   * Connect the Cathode (short leg) of the **LED** through the `220Ω` resistor to `GND`.

---

## 💻 Firmware / Code

The control logic establishes an inverted proportional relationship. It reads a 10-bit value (`0-1023`) from the LDR and maps it to an 8-bit output (`255-0`) for the PWM pin. 

```cpp
// Adaptive Smart Lighting System - Core Logic
const int sensorPin = A0;  // LDR voltage divider input
const int ledPin = 9;      // PWM output to LED

void setup() {
  pinMode(ledPin, OUTPUT);
  Serial.begin(9600);      // Initialize serial monitor for debugging
}

void loop() {
  // 1. Read ambient light (0 = total darkness, 1023 = maximum brightness)
  int photosensor = analogRead(sensorPin);
  
  // Print values to Serial Monitor for calibration
  Serial.print("Sensor Value: ");
  Serial.println(photosensor);
  
  // 2. Map the 10-bit input to an 8-bit inverted PWM output
  // As it gets darker (sensor approaches 0), brightness approaches 255
  int ledBrightness = map(photosensor, 0, 1023, 255, 0); 
  
  // 3. Drive the LED
  analogWrite(ledPin, ledBrightness);
  
  // Small delay for system stability
  delay(10); 
}
