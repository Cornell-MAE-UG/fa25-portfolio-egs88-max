---
layout: project
title: Mechanism Design with Linear Actuator
description: Homework Assignment
technologies: [n/a]
image: /assets/images/actuator mechanism.png
---
## Step 1 — Rigid Bar Mechanism Design (Updated from HW5)

### Step 1(a) — Problem, constraints/objective, and design DOF

#### Problem statement
Design a planar frame/mechanism inside a **150 cm (x) × 50 cm (y)** design space using:
- **one rigid bar** (fixed length, chosen by me),
- **3 pin supports** (exactly **two pinned to ground**),
- **one linear actuator** (selected from the provided catalog, using **max force values**),
to lift the **maximum possible weight** to the **highest possible height**, assuming all supports and the actuator are rigid.

#### Constraints
- Entire mechanism must fit within the **150 cm × 50 cm** design box during motion.
- Exactly **3 pins total**:
  - 2 ground-mounted pins
  - 1 pin connecting actuator to the bar
- Actuator must satisfy:
  - **Force:** \(F \le F_{\max}\) from the catalog (“Force up to”)
  - **Stroke:** required change in actuator length must be \(\le\) catalog stroke

#### Objective
Maximize:
- lifted load \(W\),
- vertical lift height \(h\) of the load point.

#### Design degrees of freedom (DOF)
- Bar length \(L\)
- Ground pin locations \(A(x_A,0)\), \(C(x_C,0)\)
- Actuator attachment location on bar (selected here as the tip, \(D=B\))
- Start and end bar angles \(\phi_0,\phi_1\) (set by box constraints)
- Actuator selection from catalog

---

### Step 1(b) — Static analysis (rigid bar)

#### Final chosen geometry (fits the 150 cm × 50 cm box)
- **Bar length:** \(\boxed{L=1.50\text{ m}}\)
- **Ground pin (bar pivot):** \(\boxed{A=(0,0)}\)
- **Ground pin (actuator base):** \(\boxed{C=(1.50,0)}\)
- **Actuator attaches at the bar tip:** \(\boxed{D=B}\)

Bar tip position:
\[
B(\phi)=\big(L\cos\phi,\;L\sin\phi\big)
\]

To reach the maximum allowable height (50 cm):
\[
L\sin\phi_1 = 0.50 \Rightarrow \sin\phi_1=\frac{0.50}{1.50}=\frac13
\Rightarrow \boxed{\phi_1=19.47^\circ}
\]

To avoid a degenerate (zero-length) actuator configuration, I used a small starting angle:
\[
\boxed{\phi_0=2^\circ}
\]

This keeps the tip inside the design box over the lift:
- At \(\phi_0=2^\circ\): \(B\approx(1.499,\;0.052)\) m
- At \(\phi_1=19.47^\circ\): \(B\approx(1.414,\;0.500)\) m

#### Actuator selection (catalog)
To maximize the liftable load, I selected the actuator with the largest maximum force:
\[
\boxed{F_{\max}=294\text{ kN}} \quad (\text{RSX})
\]

#### Force balance / moment balance about the pivot
The load \(W\) acts vertically downward at the tip \(B\). The actuator applies force \(F\) along the line from \(B\) to \(C\).
Taking moments about \(A\), only the tip forces contribute:
\[
F\,L\sin\theta = W\,L\cos\phi
\Rightarrow
\boxed{W = F\,\frac{\sin\theta}{\cos\phi}}
\]
where \(\theta\) is the angle between the actuator and the bar.

For this geometry (actuator from \(C\) to tip \(B\)), the angle relationship simplifies to:
\[
\boxed{\sin\theta=\cos(\phi/2)}
\]
so
\[
\boxed{W = F\,\frac{\cos(\phi/2)}{\cos\phi}}
\]

The governing (worst-case) configuration is near the start of motion. At \(\phi_0=2^\circ\),
\[
\frac{\cos(\phi/2)}{\cos\phi}
=
\frac{\cos(1^\circ)}{\cos(2^\circ)}
\approx 1.00046
\]
Thus the maximum load is essentially limited by the actuator force:
\[
\boxed{W_{\max}\approx 1.00046\,F_{\max}\approx 294\text{ kN}}
\]
(approximately the same as the actuator rating).

#### Stroke check (required actuator stroke)
Actuator length is the distance between \(C\) and \(B(\phi)\):
\[
\ell(\phi)=\sqrt{(L-L\cos\phi)^2+(0-L\sin\phi)^2}
\]

Compute at the endpoints:
- \(\ell(\phi_0)=\ell(2^\circ)\approx 0.0524\text{ m}\)
- \(\ell(\phi_1)=\ell(19.47^\circ)\approx 0.5073\text{ m}\)

Required stroke:
\[
\boxed{s_{\text{req}}=\ell(\phi_1)-\ell(\phi_0)\approx 0.455\text{ m}}
\]

This satisfies the RSX stroke capacity (“up to 1.5 m”):
\[
\boxed{s_{\text{req}} = 0.455\text{ m} \le 1.5\text{ m}}
\]

---

### Step 1(c) — Final mechanism design (drawing)

![Final mechanism layout (A, B, C and actuator)]({{ "/assets/images/Mechanism Layout.png" | relative_url }}){: .inline-image-r style="max-width: 520px"}

**Design labels to include on the figure:**
- \(A=(0,0)\), \(C=(1.50,0)\)
- \(L=1.50\text{ m}\)
- Motion range: \(\phi_0=2^\circ\rightarrow \phi_1=19.47^\circ\)
- \(F_{\max}=294\text{ kN}\) (RSX)
- \(W_{\max}\approx 294\text{ kN}\)
- Stroke: \(s_{\text{req}}\approx 0.455\text{ m}\)

> *Figure: Final rigid-bar mechanism used for maximum load and maximum lift height within the 150 cm × 50 cm design space. The RSX actuator provides the limiting force capacity, and the motion reaches the maximum allowable height of 50 cm while satisfying stroke constraints.*

## Step 2 — Bar Flexibility: Beam/Deflection Analysis (Updated from HW5)

### Given geometry (from Step 1 final design)
- Design box: 150 cm (x) × 50 cm (y)
- Bar length: **L = 1.50 m**
- Ground pins: **A = (0, 0)**, **C = (1.50, 0)** (meters)
- Actuator attaches at the bar tip: **B = D**
- Lift range: **φ₀ = 2° → φ₁ = 19.47°** (so the tip reaches **y = 0.50 m**)

---

### Step 2(a) — Maximum deflection of the bar (transverse-force model)

In Step 1, the load is applied at the tip **B** and the actuator is also connected at **B**. For Step 2, I considered only the components of the weight and actuator forces that are **transverse to the bar**.

- Transverse component of the weight:
\[
W_\perp = W\cos\phi
\]

- Transverse component of the actuator force:
\[
F_\perp = F\sin\theta
\]

Because both transverse forces act at the same location (the bar tip), moment equilibrium about the pivot **A** requires the transverse components to balance:
\[
(F_\perp - W_\perp)L = 0 \quad \Rightarrow \quad F_\perp = W_\perp
\]

Therefore, the net transverse shear and bending moment along the bar are approximately zero:
\[
V(x)\approx 0,\qquad M(x)\approx 0
\]

**Conclusion:** bending-driven deflection from transverse forces is negligible for this configuration; the dominant deformation mode is **axial compression (shortening)** of the bar.

---

### Step 2(b) — Beam design to keep vertical deflection < 2% of bar length (mass-efficient)

Although bending deflection is negligible, the bar experiences **axial compression**, which causes a small shortening \(\Delta L\). This shortening produces a vertical tip deflection:
\[
\delta_y \approx \Delta L\,\sin\phi
\]

Axial shortening for a uniform bar:
\[
\Delta L = \frac{N L}{A E}
\]

#### Maximum axial force in the bar
The internal axial force was found by projecting the external forces at the tip onto the bar axis. At the final angle (\(\phi_1=19.47^\circ\)) under the max actuator case (RSX, \(F_{\max}=294\text{ kN}\)), the maximum compressive axial force is:
\[
N_{\max} \approx 0.496F_{\max} = 0.496(294\text{ kN}) \approx \mathbf{146\text{ kN}}
\]

#### Choose a mass-efficient cross-section and material
I selected a thin-walled tube to maximize stiffness per mass.

**Material:** 6061-T6 Aluminum (\(E=69\text{ GPa}\))  
**Cross-section:** round tube  
- Outer diameter: \(D_o = 100\text{ mm} = 0.10\text{ m}\)  
- Wall thickness: \(t = 1.5\text{ mm} = 0.0015\text{ m}\)  
- Inner diameter: \(D_i = D_o - 2t = 0.097\text{ m}\)

Cross-sectional area:
\[
A = \frac{\pi}{4}(D_o^2 - D_i^2)
= \frac{\pi}{4}\left[(0.10)^2 - (0.097)^2\right]
= 4.64\times 10^{-4}\text{ m}^2
\]

Axial shortening at \(N_{\max}\):
\[
\Delta L
= \frac{N_{\max}L}{AE}
= \frac{(1.46\times 10^5)(1.50)}{(4.64\times 10^{-4})(69\times 10^9)}
= 6.8\times 10^{-4}\text{ m}
= \mathbf{0.68\text{ mm}}
\]

Vertical deflection at \(\phi_1\) (where \(\sin\phi_1 = 1/3\)):
\[
\delta_y
= \Delta L\sin\phi_1
= (0.68\text{ mm})(0.334)
= \mathbf{0.23\text{ mm}}
\]

Deflection requirement:
\[
\delta_y \le 0.02L = 0.02(1.50) = 0.03\text{ m} = 30\text{ mm}
\]

**Check:** \(\;0.23\text{ mm} \ll 30\text{ mm}\) ✅

**Conclusion:** the selected thin-walled aluminum tube satisfies the 2% deflection constraint by a large margin while keeping mass low.

---

### Step 2(c) — Final beam design (drawing + annotation)

![Final beam cross-section and key dimensions]({{ "/assets/images/Beam Design.png" | relative_url }}){: .inline-image-r style="max-width: 420px"}

**Final Beam Design (most mass-efficient meeting deflection limit):**
- **Material:** 6061-T6 Aluminum
- **Length:** \(L = 1.50\text{ m}\)
- **Cross-section:** round tube
- **Outer diameter:** \(D_o = 100\text{ mm}\)
- **Wall thickness:** \(t = 1.5\text{ mm}\)

**Key results to annotate on the figure:**
- \(N_{\max} \approx 146\text{ kN}\) (compression)
- \(\Delta L \approx 0.68\text{ mm}\)
- \(\delta_y \approx 0.23\text{ mm} < 0.02L = 30\text{ mm}\)

> *Figure: Final beam design and deflection check. Because the actuator and load act at the tip, transverse components balance and bending is minimized; axial compression governs deflection and remains far below the 2% limit.*


![Image of catalog table used to select actuator]({{ "/assets/images/actuator catalog.png" | relative_url }}){: .inline-image-r style="width: 200px"}

Here is my work from the assignment:


![My Work]({{ "/assets/images/actuator mechanism.png" | relative_url }}){: .inline-image-l}

