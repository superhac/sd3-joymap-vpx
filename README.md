Helps figure out your joystick mappings for use with vpinball.  Provides the VPX logical mappings.

<img width="435" height="649" alt="image" src="https://github.com/user-attachments/assets/2b6f4228-99bc-4763-82eb-38e2dc95b9de" />

From VPinballX.ini:
```
Device.SDLJoy_030081b85e0400008e02000014010000.Name = Xbox 360 Controller #028e
Device.SDLJoy_030081b85e0400008e02000014010000.Element0 = 512;A;Left Stick X
Device.SDLJoy_030081b85e0400008e02000014010000.Element1 = 513;A;Left Stick Y
Device.SDLJoy_030081b85e0400008e02000014010000.Element2 = 514;A;Left Trigger
Device.SDLJoy_030081b85e0400008e02000014010000.Element3 = 515;A;Right Stick X
Device.SDLJoy_030081b85e0400008e02000014010000.Element4 = 516;A;Right Stick Y
Device.SDLJoy_030081b85e0400008e02000014010000.Element5 = 517;A;Right Trigger
Device.SDLJoy_030081b85e0400008e02000014010000.Element6 = 0;B;A
Device.SDLJoy_030081b85e0400008e02000014010000.Element7 = 1;B;B
Device.SDLJoy_030081b85e0400008e02000014010000.Element8 = 2;B;X
Device.SDLJoy_030081b85e0400008e02000014010000.Element9 = 3;B;Y
Device.SDLJoy_030081b85e0400008e02000014010000.Element10 = 6;B;Back
Device.SDLJoy_030081b85e0400008e02000014010000.Element11 = 8;B;Guide
Device.SDLJoy_030081b85e0400008e02000014010000.Element12 = 7;B;Start
Device.SDLJoy_030081b85e0400008e02000014010000.Element13 = 9;B;Left Stick
Device.SDLJoy_030081b85e0400008e02000014010000.Element14 = 10;B;Right Stick
Device.SDLJoy_030081b85e0400008e02000014010000.Element15 = 4;B;Left Shoulder
Device.SDLJoy_030081b85e0400008e02000014010000.Element16 = 5;B;Right Shoulder
Device.SDLJoy_030081b85e0400008e02000014010000.Element17 = 258;B;DPad Up
Device.SDLJoy_030081b85e0400008e02000014010000.Element18 = 259;B;DPad Down
Device.SDLJoy_030081b85e0400008e02000014010000.Element19 = 256;B;DPad Left
Device.SDLJoy_030081b85e0400008e02000014010000.Element20 = 257;B;DPad Right
```

Compile:
```
sudo apt install libsdl3-dev libncurses-dev libncursesw5-dev
gcc sdl3-joymap-vpx.c -o sdl3-joymap-vpx `pkg-config --cflags --libs sdl3 ncursesw`
```

## Usage
```
./sdl3-joymap-vpx [OPTIONS]
```

### Options
| Option | Description |
|--------|-------------|
| `-l`, `--list` | List all available joystick devices and exit |
| `-j N`, `--joystick N` | Select joystick by index (default: 0) |
| `-h`, `--help` | Show help message |

### Examples
```bash
# List all connected joysticks
./sdl3-joymap-vpx -l

# Use the second joystick (index 1)
./sdl3-joymap-vpx -j 1

# Show help
./sdl3-joymap-vpx --help
```

# VPinball 10.8.1 – Input Mapping Format (Keyboard / Mouse / Joystick)

This document explains the **new unified input system** introduced in **VPinball 10.8.x**, replacing the legacy hard-coded keyboard and joystick mappings with a flexible **device + element** abstraction.

This format is designed for modern setups including:

* Multiple controllers
* Pinball cabinets
* VR
* Analog plungers
* Cross-platform (Windows / Linux / macOS)

---

## 1. High-Level Overview

### New System (10.8.1+)

* Explicit device registration
* SDL-based joystick identification
* Stable GUID-based device binding
* Logical **elements** instead of raw button numbers
* Multiple devices per action

> **Core idea:**
>
> **Action → Device → Element**

---

## 2. `[Input]` Section Structure

```ini
[Input]
Devices = Key;Mouse;SDLJoy_030081b85e0400008e02000014010000
```

The `Devices` entry defines the **ordered list of active input devices**.

### Device Identifiers

| Identifier      | Description            |
| --------------- | ---------------------- |
| `Key`           | Keyboard               |
| `Mouse`         | Mouse                  |
| `SDLJoy_<GUID>` | SDL joystick / gamepad |

The joystick GUID is provided by SDL and uniquely identifies the physical device:

```
030081b85e0400008e02000014010000
```

This ensures the same controller is matched reliably across reboots and platforms.

---

## 3. Device Types

```ini
Device.Key.Type = 1
Device.Mouse.Type = 3
Device.SDLJoy_....Type = 2
```

Internal device type enumeration:

| Type | Meaning                  |
| ---: | ------------------------ |
|    1 | Keyboard                 |
|    2 | Joystick / Gamepad (SDL) |
|    3 | Mouse                    |

---

## 4. `NoAutoLayout`

```ini
Device.SDLJoy_....NoAutoLayout = 1
```

Controls whether VPinball automatically assigns default mappings.

|        Value | Behavior                 |
| -----------: | ------------------------ |
| `0` or empty | Auto-map defaults        |
|          `1` | **Disable auto-mapping** |

For cabinet and custom controller setups, this should typically be set to `1`.

---

## 5. Device Names

```ini
Device.SDLJoy_....Name = Xbox 360 Controller #028e
```

* Descriptive only
* Used for UI display
* Does not affect behavior

---

## 6. Input Elements

Each device exposes a set of **elements**, which represent the smallest bindable inputs.

### Element Definition Format

```ini
Device.<DeviceID>.ElementN = <code>;<type>;<label>
```

### Example

```ini
Device.SDLJoy_....Element6 = 0;B;A
```

| Field | Description          |
| ----: | -------------------- |
|   `0` | SDL element ID       |
|   `B` | Element type         |
|   `A` | Human-readable label |

---

## 7. Element Types

### Buttons (`B`)

Digital on/off inputs:

```ini
6;B;A
7;B;B
258;B;DPad Up
```

Includes:

* Face buttons
* Shoulder buttons
* D-Pad directions
* Stick clicks

---

### Axes (`A`)

Analog inputs:

```ini
512;A;Left Stick X
513;A;Left Stick Y
514;A;Left Trigger
```

Used for:

* Analog plunger position / velocity
* Nudging
* VR movement
* Analog flippers

> **Note:** Triggers are exposed as axes by SDL.

---

## 8. Mapping Entries

```ini
Mapping.LeftFlipper =
Mapping.RightFlipper =
Mapping.Pl
```

## 9. The ; (semicolon) — Field separator

Each binding is a semicolon-delimited mini-structure:

`<Device> ; <ControlID> ; <Type> ; <Param1> ; <Param2> ; <Param3>`

Not all fields are required.

## 9.1 Keyboard bindings
`Key;225`

- Key → keyboard device

- 225 → internal VPX key code (not ASCII, not SDL)

Example:

`Mapping.Start = Key;30`


“Start is triggered by key code 30”

## 9.2 SDL joystick bindings
`SDLJoy_<GUID> ; <ControlID> ; <Type> ; <Params...>`

Example:

`SDLJoy_030081b85e0400008e02000014010000;514;o;-0.300000`


Breakdown:
- SDLJoy_<GUID> → specific joystick (GUID from SDL)
- 514 → axis/button ID
- o → one-shot analog trigger
- -0.300000 → threshold

## 10. Control IDs (the numbers)
Ranges
|Range	| Meaning |
|-------|---------|
|0–127	| Buttons |
|256–511 | 	Hats / D-pad |
| 512+	 | Axes   |

Examples:
- 0, 1, 3 → joystick buttons
- 256–259 → service switch hat inputs
- 512, 514, 516, 517 → analog axes

## 11. Analog types (o, P, etc.)
— One-shot analog trigger

Used for staged flippers and axis-based buttons.

`SDLJoy_...;514;o;-0.300000`

Meaning:
-When axis 514 crosses -0.3, fire once
- Used for:
  - Flipper press
  - EOS/staged flippers

You’ll see pairs:

```
-0.300000   (normal flipper)
+0.300000   (staged flipper)
```

— Proportional axis

Used for continuous analog values

```
Mapping.PlungerPos =
  SDLJoy_...;516;P;0.000000;1.000000;1.000000
```

Fields:

`P ; min ; max ; scale`


Meaning:
- Axis value maps from 0.0 → 1.0
- Scaled linearly
- Used for:
   - Plunger position
   - Nudging

# Appendix
[SDL3 Scan Codes](https://github.com/libsdl-org/SDL/blob/main/include/SDL3/SDL_scancode.h)
