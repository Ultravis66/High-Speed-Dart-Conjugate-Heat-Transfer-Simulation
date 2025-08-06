# 🚀 High-Speed Dart: Conjugate Heat Transfer Simulation
**Inspired by real-world subcaliber designs**

This project models the **transient thermal response** of a multi-material, high-speed dart as it decelerates from **Mach 4 to 3.5 over 2 seconds** in standard atmosphere. Using **conjugate heat transfer (CHT)** and a custom **quasi-unsteady macro**, it captures aerodynamic heating and internal conduction through layered solids: tungsten, epoxy, aluminum, and steel.

---

## 📂 Geometry & Setup

- **HighSpeed DartFullGeometry.png**: full geometry used in simulation  
- **Materials.png**: cutaway close-up showing layered nose (steel, aluminum, epoxy, tungsten)  
- **Mesh.png**: solid/fluid mesh structure  
- **Geometry source**: [30mm NATO APFSDS on GrabCAD](https://grabcad.com/library/30mm-nato-apfsds-1)  
> Dimensions and material layering were modified based on open-source knowledge of subcaliber darts. This model is used purely for physics-based simulation and does not reflect proprietary or classified designs.

---

## 🧪 Simulation Method

### Quasi-Unsteady CHT Modeling

- The simulation alternates between **fluid and solid phases**, stepping time forward gradually
- A custom Java macro (`SolidFluidAlternatingMacro`) manages:
  - Activation/deactivation of physics continua
  - Time advancement logic
  - Fluid: steady runs in short bursts
  - Solid: transient heat soak integration  
- Velocity decay from Mach 4 → 3.5 modeled using:  
  ```
  M(t) = 4 * exp(-0.06615 * t)
  ```

---

### 🔄 Macro Overview (Excerpt)
```java
while (currentTime < endTime) {
    fluidCont.setIsActive(true);    // fluid phase
    [solids off...]
    sim.run();

    fluidCont.setIsActive(false);   // solid phase
    [solids on...]
    timeStopCrit.setMaximumTime(...);
    sim.run();

    currentTime = sim.getSolution().getPhysicalTime();
}
```

---

## 📊 Results

## 🧪 1D Heat Transfer Validation

To validate the solver accuracy, a simple **1D conduction case** was extracted at a depth of **1 mm** into the steel tip. This was compared against the analytical solution for semi-infinite conduction using the **error function (erf)** solution:

\[
T(x, t) = T_\infty + (T_s - T_\infty) \cdot \text{erf} \left( \frac{x}{2\sqrt{\alpha t}} \right)
\]

Where:
- \( x = 1 \text{ mm} \)  
- \( \alpha \) = thermal diffusivity of steel  
- \( T_s \) = surface temperature  
- \( T_\infty \) = initial steel temperature

The plot below shows excellent agreement between the numerical simulation and the analytical curve:

![1Dheat transfer](1Dheat%20transfer.png)
## 🌡️ Epoxy Layer Temperature Over Time

This plot shows the **temperature evolution** inside the **epoxy bonding layer** over the full 2-second simulation window. The data was extracted from a probe located near the mid-thickness of the epoxy region, at the nose-body interface between the steel tip and the aluminum outer shell.

This temperature trace highlights:
- Rapid heating in the first 0.2–0.4 seconds due to sharp temperature gradients
- Gradual thermal response lagging behind steel and aluminum due to lower thermal conductivity
- Long tail response as energy conducts inward toward the tungsten core

![Epoxy_Temp](Epoxy_Temp.png)


## 🔥 Full Temperature Field Visualization (With and Without Mesh)

These visualizations show the **temperature field** across all solid materials at a representative moment during the simulation.

### With Mesh Overlay
The image below includes the **solid region mesh**, helping illustrate:
- How conduction pathways evolve from the **steel tip inward**
- Layered heat transfer behavior through **aluminum**, **epoxy**, and into the **tungsten core**
- Mesh resolution and cell distribution in critical thermal gradient regions

![Temps](Temps.png)

### Without Mesh Overlay
The same temperature field is shown below **without mesh lines** for a clearer, presentation-quality view of thermal distribution. This version highlights:
- Temperature gradient smoothness
- Thermal interfaces between materials
- Direction of heat flow and dominant conduction regions

![Temps2](Temps2.png)
---

## 📽️ Temperature Evolution (2-Second Transient)

This video visualizes the **conjugate heat transfer behavior** of the dart geometry over a full **2-second flight window**, during which the Mach number decays from 4.0 to 3.5. The animation shows:

- Rapid **heating of the steel tip**
- **Boundary layer temperature rise** around the nose and body
- Gradual **conduction into aluminum, epoxy, and tungsten**
- Layer-by-layer heat propagation through the solid core

[🎬 Watch temp.mp4](Temp.mp4)

---

## ⚠️ Disclaimer
This project is intended for **open-source educational and research purposes only**. It is not a representation of any classified or proprietary system. Geometry was adapted from publicly available sources and modified extensively for thermal physics analysis.
