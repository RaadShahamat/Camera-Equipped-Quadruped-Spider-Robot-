# Camera-Equipped-Quadruped-Spider-Robot-
Author - Raad Shahamat Alif


This project is an **Arduino sketch** providing the foundational control logic for an **8-DOF (Degrees of Freedom) Quadruped Robot** (spider-bot). It enables the robot to execute basic movements, special actions, and adjust its stance, all driven by serial commands (typically from a Bluetooth module or similar wireless link).

***

## 🌟 Key Features and Integrated Modules

The design is modular, separating the main loop, movement execution, and low-level servo control.

### 1. ⚙️ Core Hardware & Control Module
* **8-DOF Design:** Controls **eight hobby servo motors** (two per leg: one for **Pivot** and one for **Lift**), allowing for complex, multi-joint leg movements essential for effective walking gaits.
* **Pin Configuration:** Servos are mapped to Arduino digital pins **4 through 11**.
* **Real-Time Calibration:** Includes **global offsets** (`da`, `db`, `dc`, `dd`) for the pivot servos, allowing the robot's mechanical center to be adjusted in software to compensate for physical assembly tolerances.
* **Initial State:** All servos are initialized to a **90-degree center position** (`center_servos()`) upon startup, ensuring a stable, ready-to-move posture.

### 2. 📡 Communication Module
* **Serial Input:** The robot is designed for **remote control** via **Serial Communication** (Baud Rate: **9600**).
* **Command Parsing:** The main `loop()` continuously monitors the serial port and processes single-character commands (`F`, `B`, `L`, `R`, `X`, `Y`, `S`, etc.) to trigger specific functions.

### 3. 🚶 Gait and Movement Module
This module contains the primary functions that define how the robot walks and moves, using pre-calculated servo positions for coordinated leg motion.
* **Translational Movement:**
    * **`F`** (`forward()`): Executes a coordinated **gait sequence** for continuous forward walking.
    * **`B`** (`back()`): Executes a gait sequence for backward movement.
* **Rotational Movement (Turning):**
    * **`L`** (`turn_left()`): Executes a turning gait sequence (in place or wide turn).
    * **`R`** (`turn_right()`): Executes a turning gait sequence.
* **Stop/Idle:**
    * **`S`** (Stop): Returns the robot to a centered, stable standing position and resets height/speed settings.

### 4. 🕺 Special Actions Module
The robot can execute pre-programmed, dynamic sequences for non-locomotive actions.
* **`X`** (`dance()`): A choreographed sequence of leaning and bowing motions.
* **`Y`** (`wave()`): Lifts and articulates the **Front Right Leg** for a wave action.
* **`bow()`:** Tilts the front section down.
* **`lean_left()` / `lean_right()`:** Tilts the robot's entire body for dynamic stance or expressive movement.

### 5. 🎚️ Dynamics and Low-Level Control Module
The `srv()` function is the heart of the low-level movement control, ensuring smooth and tunable execution of servo movements.
* **Smooth Motion Engine:** The `srv()` function transitions all eight servos **incrementally** from their current angles to the target angles. This prevents jerky motion and simulates inertia.
* **Speed Control (`spd`):** The global variable `spd` acts as the primary delay in the `srv()` loop. **A higher value results in slower, smoother motion.**
* **Height Adjustment (`high`):** The global variable `high` (default 15) is used to scale the **Lift Servo** target positions (`p12`, `p22`, etc.), allowing the operator to adjust the robot's ground clearance and standing height dynamically.

***

## 📋 Command Protocol Summary

| Command | Function Call | Description |
| :---: | :--- | :--- |
| **`F`** | `forward()` | Move the robot **forward**. |
| **`B`** | `back()` | Move the robot **backward**. |
| **`L`** | `turn_left()` | **Turn left**. |
| **`R`** | `turn_right()` | **Turn right**. |
| **`X`** | `dance()` | Execute a **dance** sequence. |
| **`Y`** | `wave()` | Execute a **wave** with the front right leg. |
| **`S`** | `center_servos()` | **Stop** and stabilize the stance. |
| **`M`**, **`m`**, **`N`**, **`n`** | *(Currently unused but reserved)* | Placeholder commands for potential future controls (e.g., speed, height adjustment). |
