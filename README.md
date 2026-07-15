# MATLAB/Simulink Simulation & Analytical Verification of a 3-Phase Induction Motor

This repository contains the modeling, simulation, and theoretical validation of a three-phase Asynchronous Induction Motor[cite: 4]. The project covers steady-state performance characteristics, transient emergency braking (plugging dynamics), and speed/torque control of a wound-rotor machine[cite: 4, 5].

This project was conducted as part of the **Electrical Machines II** curriculum at Shiraz University[cite: 2, 4].

### 📋 Project Metadata
* **Author:** Hossein Davoodi[cite: 2, 4]
* **Institution:** Shiraz University[cite: 2, 4]
* **Course Instructor:** Dr. Mohammad Rastegar[cite: 2, 4]
* **Term:** Fall 2021 (1400)[cite: 2, 4]

---

## 🛠️ Machine Specifications & Parameters

The simulation model is built using an asynchronous machine preset with the following electrical ratings[cite: 4]:

* **Rated Active Power ($P_{\text{rated}}$):** 4 kW (5.4 HP)[cite: 2, 4]
* **Rated Line Voltage ($V_{LL}$):** 400 V (Star-Connected, Phase Voltage $V_{ph} = 230.94\text{ V}$)[cite: 4]
* **Rated Frequency ($f_s$):** 50 Hz[cite: 4]
* **Rated Mechanical Speed ($N_{\text{rated}}$):** 1430 RPM[cite: 4]
* **Equivalent Circuit Parameters (from preset model parameters):**[cite: 4]
  * Stator Resistance ($R_s$): 1.405 Ohm[cite: 4]
  * Rotor Resistance ($R_r'$): 1.395 Ohm[cite: 4]
  * Stator Leakage Reactance ($X_s$): 1.833 Ohm[cite: 4]
  * Rotor Leakage Reactance ($X_r'$): 1.833 Ohm[cite: 4]
  * Magnetizing Reactance ($X_m$): 54.07 Ohm[cite: 4]

---

## 🚀 Technical Implementation & Key Results

### 1. Steady-State Characterization & Simulation Design
Direct startup of an induction motor introduces significant transient currents and torque spikes[cite: 4]. To bypass these initial oscillations and cleanly record the steady-state torque-speed ($\tau-\omega$) characteristic curve[cite: 4]:
* **Ramp-Controlled Speed:** The mechanical rotor speed input was driven by a Ramp block with a start time of 2 seconds and a slope of 60[cite: 4].
* **Transient Isolation:** A Step block triggered a Sample and Hold (S/H) block at exactly $t = 2\text{ s}$[cite: 4]. This ignored the initial startup transients, logging only clean steady-state data[cite: 4].
* **Data Extraction:** A Bus Selector isolated "Rotor speed" and "Electromagnetic torque" to map them directly to the XY Graph plot[cite: 3, 4].

#### Complete Torque-Speed Characteristic Plot (Motoring vs. Generating)
The simulation cleanly captures both motoring and regenerating regions[cite: 4]. In motoring mode, torque peaks at $T_{\text{max}} = 92.5\text{ N}\cdot\text{m}$ before dropping to zero at synchronous speed (1500 RPM)[cite: 4]. Operating the machine above synchronous speed (negative slip) turns it into an induction generator (returning power to the grid, represented by negative torque values)[cite: 4].

![Torque Speed Plot](images/torque_speed_curve.png)

---

### 2. Analytical Verification (Thévenin Equivalents)
To validate the Simscape model, hand calculations were executed using the classic Thévenin equivalent circuit of an induction motor[cite: 4].

#### Thévenin Parameter Derivation
The Thévenin voltage constant ($k_{th}$) is defined as[cite: 4]:
$$k_{th} = \frac{X_m}{X_m + X_s} = \frac{54.07}{54.07 + 1.833} \approx 0.9672$$

Applying this to the phase input voltage ($V_{ph} = 230.94\text{ V}$)[cite: 4]:
$$V_{th} = V_{ph} \cdot k_{th} = 230.94\text{ V} \cdot 0.9672 \approx 223.366\text{ V}$$

The Thévenin stator resistance ($R_{th}$) and leakage reactance ($X_{th}$) are[cite: 4]:
$$R_{th} \approx R_s \cdot k_{th}^2 = 1.405\ \Omega \cdot (0.9672)^2 \approx 1.3143\ \Omega$$
$$X_{th} \approx X_s \approx 1.7733\ \Omega$$

For a 4-pole machine connected to a 50 Hz supply, the synchronous angular velocity ($\omega_s$) is[cite: 4]:
$$\omega_s = \frac{2\pi \cdot 120 f}{60 \cdot P} = \frac{2\pi \cdot (2 \cdot 50)}{4} = 157\text{ rad/s}$$

#### Peak Torque ($T_{\text{max}}$) and Starting Torque ($T_{\text{st}}$) Computations
$$T_{\text{max}} = \frac{3 |V_{th}|^2}{2 \omega_s \left( R_{th} + \sqrt{R_{th}^2 + (X_{th} + X_r')^2} \right)}$$

$$T_{\text{max}} = \frac{3 \times (223.366\text{ V})^2}{2 \times 157\text{ rad/s} \left( 1.3143\ \Omega + \sqrt{1.3143^2 + (1.7733 + 1.833)^2}\ \Omega \right)} \approx 92.5\text{ N}\cdot\text{m}$$

$$T_{\text{st}} = \frac{3 \cdot R_r' |V_{th}|^2}{\omega_s \left( (R_{th} + R_r')^2 + (X_{th} + X_r')^2 \right)}$$

$$T_{\text{st}} = \frac{3 \times 1.395\ \Omega \times (223.366\text{ V})^2}{157\text{ rad/s} \left( (1.3143 + 1.395)^2 + (1.7733 + 1.833)^2 \right)\ \Omega^2} \approx 59\text{ N}\cdot\text{m}$$

The simulation-derived parameters match these mathematical results with 100% precision, validating the integrity of the Simscape modeling environment[cite: 4].

---

### 3. Transient Plugging (Phase-Reversal Emergency Braking)
Plugging is a highly transient braking method modeled by reversing two stator supply phases during normal motor operation, creating a rotating stator field that spins in opposition to the rotor[cite: 4].

* **Slip Step-Change:** During normal run, slip is minimal ($s \approx 0.05$)[cite: 4]. Upon phase swap, the rotating field reverses synchronously relative to the rotor speed ($N_m \approx N_s$), causing the slip to jump immediately to[cite: 4]:
  $$s = \frac{-N_s - N_m}{-N_s} \approx 2$$
* **Deceleration Pathway:** The negative torque acting on the rotor rapidly decelerates the shaft speed down to zero ($s = 1$)[cite: 4]. If the supply is not disconnected immediately at zero speed, the rotor naturally begins accelerating in the reverse direction up to synchronous speed in reverse ($s \approx 0$)[cite: 4].
* **Rotor Frequency Transient:** The rotor current frequency varies dynamically as a function of slip ($f_r = s \cdot f_s$)[cite: 4]. In plugging, this frequency spikes close to 100 Hz ($s \approx 2$), resulting in highly elevated rotor currents and high transient thermal stresses[cite: 4].

---

### 4. Wound-Rotor Speed & Torque Control
To verify wound-rotor speed-torque control, star-connected external resistors ($R_{ext}$) were connected to the rotor winding slip rings[cite: 5]. The external resistances were stepped incrementally from $0.1\ \Omega$ to $1.7\ \Omega$ in $0.4\ \Omega$ steps[cite: 5].

| $R_{\text{ext}} = 0.1\ \Omega$ | $R_{\text{ext}} = 0.5\ \Omega$ |
| :---: | :---: |
| ![R = 0.1](images/r_01.png) | ![R = 0.5](images/r_05.png) |

| $R_{\text{ext}} = 0.9\ \Omega$ | $R_{\text{ext}} = 1.3\ \Omega$ |
| :---: | :---: |
| ![R = 0.9](images/r_09.png) | ![R = 1.3](images/r_13.png) |

| $R_{\text{ext}} = 1.7\ \Omega$ | |
| :---: | :---: |
| ![R = 1.7](images/r_17.png) | |

#### Parametric Analysis
The simulation runs successfully demonstrate key machine physics[cite: 5]:
1. **Starting Torque ($T_{st}$):** At startup (speed = 0), the torque scales up directly as external resistance increases, confirming why wound-rotor machines are selected for heavy, high-inertia industrial loads[cite: 5].
2. **Breakdown Torque ($T_{max}$):** The maximum torque remains constant in magnitude (at approximately 45.8 N·m) but shifts progressively to lower speeds (higher slip values)[cite: 5].
