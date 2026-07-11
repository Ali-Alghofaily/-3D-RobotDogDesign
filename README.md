# -3D-RobotDogDesign
this is a 3D-RobotDogDesign for my summer training task at Smart Methods

# 3D Robot Quadruped Design (Tinkercad Initial Prototype)

This repository contains the documentation and design details for an initial mechanical prototype of a quadruped (four-legged) robot dog. The design was built using **Autodesk Tinkercad**, focusing on fundamental mechanical engineering concepts to enable static stability and basic walking capabilities.

## 🛠️ Design Overview

The design represents a highly structured, symmetrical approach to building a foundational quadruped robot using standard, easily accessible components. 

*   **Chassis:** A spacious, lightweight modular frame featuring precise rectangular slots designed for **SG90 Micro Servo Motors**.
*   **Leg Assemblies:** A 2-joint multi-segment leg architecture designed to optimize both vertical ground clearance and forward stride efficiency.
*   **Aesthetics & Usability:** Includes realistic structural details, such as a dog-shaped headpiece, to enhance visual tracking during motion analysis.

---

## 📊 Technical Requirements & Analysis

### 1. Structure & Material Volume
*   **Body Dimensions:** Approximately $100\text{ mm} \times 28\text{ mm} \times 20\text{ mm}$ (Main plate thickness: $4\text{ mm}$).
*   **Weight Management:** The frame includes inner structural cutouts to significantly reduce the total mass (and therefore the inertia) while maintaining robust bending stiffness.

### 2. Kinematics & Degrees of Freedom (DoF)
The robot utilizes a **4 Degrees of Freedom (DoF)** or an extended **8 DoF layout** depending on the servo actuation strategy:
*   **Abduction/Adduction (Hip Roll):** Controlled by 4 horizontal servos positioned at the corners of the chassis to pivot legs outwards/inwards.
*   **Flexion/Extension (Hip Pitch):** Vertical joint linkages enabling forward and backward locomotion strokes.

### 3. Motor Selection (SG90 Micro Servo)
*   **Operating Voltage:** $4.8\text{V} - 6.0\text{V}$
*   **Stall Torque:** $1.3\text{ kg}\cdot\text{cm}$ (at $4.8\text{V}$) to $1.6\text{ kg}\cdot\text{cm}$ (at $6.0\text{V}$).
*   **Weight:** $9\text{g}$ per actuator.
*   **Justification:** Low-cost, lightweight, and natively supported by standard university microcontroller platforms (e.g., Arduino Uno / ESP32).

### 4. Mathematical Torque Calculation
To ensure the selected SG90 servos can lift or maintain the robot's posture, we calculate the worst-case holding torque at the hip joint when a leg is fully extended horizontally.

$$\text{Total Robot Weight (Estimated)} = 250\text{ g} \rightarrow m = 0.25\text{ kg}$$
$$\text{Gravitational Acceleration } (g) = 9.81\text{ m/s}^2$$
$$\text{Maximum Horizontal Extension } (d) = 5.5\text{ cm} = 0.055\text{ m}$$

$$\text{Torque } (\tau) = F \times d = (m \times g) \times d$$
$$\tau = (0.25 \times 9.81) \times 0.055 = \mathbf{0.1349\text{ N}\cdot\text{m}}$$

Converting Newtons-meters to kilogram-centimeters:
$$\tau \approx 0.1349 \times 10.197 = \mathbf{1.375\text{ kg}\cdot\text{cm}}$$

**Critical Summary:** Since the maximum stall torque of an SG90 servo is $1.3 - 1.6\text{ kg}\cdot\text{cm}$, the system operates right at its physical limit during full extension. To maintain reliability, the walking algorithms must keep the legs close to the body center to reduce the effective lever arm ($d$).

### 5. Center of Gravity (CoG) & Static Stability
*   **Static Balance:** The Center of Gravity (CoG) is perfectly centered on the geometric midpoint of the rectangular chassis.
*   **Support Polygon:** While standing, the four feet create a rectangular support polygon. As long as the projection of the CoG remains within this polygon, the robot is unconditionally stable.

*   ### 6. Locomotion Strategy (Crawl Gait)
To ensure safety and continuous balance, a periodic **Static Crawl Gait** is proposed:
1.  **Sequence:** Move Rear-Left $\rightarrow$ Move Front-Left $\rightarrow$ Move Rear-Right $\rightarrow$ Move Front-Right.
2.  **Rule:** At any given moment, exactly **3 feet must maintain secure ground contact**, forming a stable triangular support polygon around the CoG.

### 7. Anticipated Mechanical Issues
*   **Backlash & Play:** Cheap plastic gears inside the SG90 servos will introduce minor angular inaccuracies, leading to wobble over long periods.
*   **Structural Deflection:** 3D-printed leg extensions can flex under dynamic loads; printing with a higher infill density ($>40\%$ with cubic patterns) is highly recommended.
*   **Thermal Fatigue:** Operating close to the stall torque limit will cause the micro-servos to overheat quickly, requiring periodic cooldown cycles in code.
* 
