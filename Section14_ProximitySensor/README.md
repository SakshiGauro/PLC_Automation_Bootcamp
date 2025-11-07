# Chapter 14: Automation Proximity Sensors
This chapter explains the concept and operation of **Proximity Sensors** in industrial automation and PLC programming.

A **proximity sensor** detects the presence of an object *without physical contact*.  
It outputs a **digital signal (ON/OFF)** depending on whether an object enters the sensing range:

| State | Output Signal |
|--------|--------------|
| No object detected | `LOW (0)` |
| Object detected | `HIGH (1)` |

The PLC reads this output and performs actions based on the ladder logic program.

---

## Types of Proximity Sensor

### 1. Optical Sensor
Uses **light** (infrared/laser) to detect the presence of an object.

#### a. **Through-Beam Sensor**

Two separate modules:  
- **Transmitter (Tx)** — sends light  
- **Receiver (Rx)** — detects the transmitted light.

| State | Output |
|--------|--------|
| Light received (no object) | HIGH (1) |
| Light blocked (object detected) | LOW (0) |
 
**Sensing distance:** up to **30 m**

**How it works:**
The receiver continuously expects a light beam. When an object interrupts the beam, the signal changes.

**Pros:** 
- Longest sensing distance (up to 30m)
- Highly accurate and reliable
- Good for harsh environments: dust, oil, dirt

**Cons**
- Requires precise alignment of Tx and Rx
- Cannot detect transparent objects (light still passes through)

#### b. reflected beam
Transmitter and receiver are in the **same housing**. Uses a reflective surface or the object's surface.

| Condition | Output |
|----------|--------|
| No object detected | LOW (0) |
| Object reflects beam | HIGH (1) |

**Sensing distance:** up to **15 m**

**Details:**
- Sensor sends a beam and waits for reflection (from an object or reflector).
- Sensitivity can be adjusted based on surface color or reflectivity.

**Pros**
- Easy installation (only one device instead of two)
- Adjustable sensitivity
- Detects most solid objects

**Cons**
- Does *not* detect transparent objects reliably (light passes through)
- Affected by reflective/shiny surfaces (false positives)

**Common uses:**
- Elevator door safety detection
- Escalator/person detection to save power
- Conveyor object detection

---

### 2. Inductive Proximity Sensor

Detects **metallic objects only**.

| Condition | Output |
|----------|--------|
| No metal in range | LOW (0) |
| Metal detected | HIGH (1) |

**Sensing principle:**
- Sensor generates a high-frequency electromagnetic field using an internal coil.
- Metal within range changes the field strength.
- The sensor detects this change and outputs a signal.

**Pros**
- Immune to dust, dirt, oil, or moisture
- Very reliable in industrial environments
- Fast response and long life (no mechanical contact)

**Cons**
- Detects *only* metals (steel, iron, aluminum, etc.)
- Short sensing range (typically **up to 15 cm**)

**Common uses:**
- Metal detection in food safety systems
- Weapon detection in security systems
- Position sensing on machinery

---

### 3. Capacitive Proximity Sensor

Detects **all materials**: metal, plastic, wood, liquid, powder, etc.

| Condition | Output |
|----------|--------|
| No object detected | LOW (0) |
| Any material detected | HIGH (1) |

**Sensing principle:**
- Uses an internal capacitor sensitive to changes in electrostatic field.
- When any material enters the sensing area, capacitance increases and triggers output.

**Pros**
- Can detect **solids and liquids**
- Can detect **water level inside a tank (without contact!)**
- Adjustable sensitivity

**Cons**
- Can give false triggers due to humidity/moisture
- Shorter sensing range than optical sensors

**Common uses:**
- Level sensing (water tanks, grain silos)
- Packaging lines (detect powder or plastic pellets)

---

## How to Choose the Right Proximity Sensor

Choosing the correct proximity sensor depends on **what object you are detecting**, the **environment**, and **electrical compatibility** with the PLC.

### 1. Based on Object Type (Sensor Technology)

| Requirement / Detection Goal | Best Sensor Type | Why |
|------------------------------|------------------|-----|
| Detect **metallic objects only** | **Inductive Sensor** | Uses electromagnetic field — stable, immune to dust/oil |
| Detect **any material** (solid, liquid, powder) | **Capacitive Sensor** | Uses electrostatic field — detects anything with dielectric property |
| Detect objects **from a distance** (e.g., counting parts on conveyor) | **Optical Photoelectric Sensor** | Uses light beam — longest range |
| Detect **transparent or thin objects**, highest accuracy | **Through-Beam Optical Sensor** | Beam must break → extremely reliable |

> ✦ Rule of thumb:  
> If the object is metallic → use **Inductive**  
> If the object is non-metallic or you are unsure → use **Capacitive**  
> If the object is far → use **Optical**

### 2. Sensor Rated Voltage (Power Supply Compatibility)
Sensors come in different voltage ranges:

| Voltage Type | Typical Use Case |
|--------------|------------------|
| **12–24 VDC** | PLCs, robotics, industrial machines (most common) |
| **110–220 VAC** | Large industrial equipment, legacy automation panels |

> Choose a sensor that matches your **PLC input module voltage** (usually 24V DC).

If your PLC input module accepts **24V DC**, do **not** use a 220V AC proximity sensor without electrical protection.

### 3. Sensing Range (Distance)

- Inductive sensors → **short range** (typically 2–15 cm)
- Capacitive sensors → similar range (depends on material density)
- Optical sensors → **longest range** (up to 30 meters)

> A sensor must be selected with enough range to **reliably detect the object even if installation alignment changes**.

### 4. Wiring and connection (PNP vs NPN)

Different proximity sensors output the signal differently:

#### a. PNP sensor (Sourcing Output) — PLC receives +24V when ON**

Sensor **supplies** current → pushes voltage to the PLC input.

| Wire Color | Function |
|------------|----------|
| Brown | +24V (Power supply +) |
| Blue | 0V (Power supply –) |
| Black | Signal wire to PLC input |

| Object State | Output |
|--------------|--------|
| No object detected | LOW (0) |
| Object detected | HIGH (1) |

➡ Most common in Europe and the U.S. industrial automation.
➡ Recommended for Allen-Bradley / Siemens PLCs.

#### b. NPN (Sinking Output) — PLC input pulled to 0V when ON

Sensor **pulls down** the signal → sinks current.

| Wire Color | Function |
|------------|----------|
| Brown | +24V (Power supply +) |
| Blue | Ground / 0V |
| Black | Signal wire to PLC input |

| Object State | Output |
|--------------|--------|
| No object detected | HIGH (1) |
| Object detected | LOW (0) |

➡ Common in Asian systems and small machinery.

> **Compatibility Rule:**
> - If the PLC input module is **sinking**, use **PNP sensor**
> - If the PLC input module is **sourcing**, use **NPN sensor**
>
> Mixing them incorrectly will result in **no detection or damaged hardware**.

### 5. Number of sensor wires (Functionality)

| Sensor Type | Description |
|-------------|-------------|
| **3-wire sensor** | Common type: +V, 0V, and either NO *or* NC output |
| **4-wire sensor** | Has **both NO and NC outputs** – more flexible in PLC programming |

> Use **4-wire** sensors when the program might require switching between normally open and normally closed logic.
