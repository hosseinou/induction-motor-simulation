# 3-Phase Induction Motor Simulation

MATLAB/Simulink models to analyze the steady-state and transient behavior of a 4kW squirrel-cage and wound-rotor induction motor. This was completed as part of the Electrical Machines II course at Shiraz University.

## Project Structure

* `IM1.slx`: Squirrel-cage motor model used for steady-state and plugging (braking) analysis.
* `wound_2.slx`: Wound-rotor motor model with external star-connected resistance loops.
* `images/`: Simulation scopes, schematics, and analytical verification sheets.

## Key Simulation Highlights

### 1. Steady-State Torque-Speed Curve
* Swept motor speed from 0 to 3000 RPM (2x synchronous speed) using a velocity ramp.
* Integrated a Sample & Hold block triggered at t = 2s to bypass startup transient oscillations, ensuring a clean steady-state plot.
* Verified simulation milestones against hand calculations (Calculated Breakdown Torque: ~92.5 N.m, Starting Torque: ~59 N.m).

### 2. Transient Plugging (Emergency Braking)
* Simulated phase-reversal braking by swapping two stator input phases during rated operation.
* Analyzed the transient behavior as slip instantly steps to s ≈ 2, forcing rapid deceleration to standstill.

### 3. Rotor Resistance Control (Wound-Rotor Model)
* Stepped external rotor resistance from 0.1 to 1.7 Ohms (0.4 Ohm steps).
* **Takeaway:** Confirmed that increasing rotor loop resistance increases starting torque without altering the peak breakdown torque magnitude, shifting the maximum torque slip point lower.

## How to Run
1. Open MATLAB and ensure Simscape Electrical / Power Systems toolbox is installed.
2. Clone this repository and open either `.slx` file.
3. Run the simulation to view the XY Graph scopes for the torque-speed trajectories.
