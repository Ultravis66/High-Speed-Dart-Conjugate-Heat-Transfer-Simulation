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

- **1Dheat transfer.png**: 1D conduction into steel tip compared to analytical erf solution at 1 mm depth  
- **Epoxy_Temp.png**: time-based temperature trace inside epoxy bond layer  
- **Temps.png**: temperature field showing conduction from steel into aluminum, epoxy, and tungsten (with mesh overlay)  
- **Temps2.png**: same view as above without mesh overlay — cleaner visual for presentation  

---

## 📽️ Coming Soon
- Video visualization of **temperature evolution** during heating  
- Optional: animation of **pressure field and shock behavior** as Mach decays

---

## ⚠️ Disclaimer
This project is intended for **open-source educational and research purposes only**. It is not a representation of any classified or proprietary system. Geometry was adapted from publicly available sources and modified extensively for thermal physics analysis.
