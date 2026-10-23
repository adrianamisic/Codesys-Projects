# Codesys-Projects

Codesys projects for the control systems lab. The tasks simulate various real-world industrial control applications.

## **1. Deicing Plant**  
   This module controls an automated aircraft deicing fluid mixing and storage system (Type IV fluid system). 

   ### System Functional Overview & Logic
   The system handles the preparation, mixing, recirculation, and distribution of Type IV aircraft deicing fluid using four 100 m³ tanks (T1 to T4), controlled via a finite state machine / Sequential Function Chart (SFC) in CoDeSys:

   * **Tank 1 (T1) - Unmixed Storage:** Receives concentrated glycol thickener mix (GKV) from tanker trucks (TKW) via pump P1.
   * **Tank 2 (T2) - Mixing Tank:** Prepares two specific dilution ratios when storage tanks (T3 or T4) drop below 40 m³:
     * **Mix 50 (50% GKV + 50% H2O):** 30 m³ GKV + 30 m³ water. Mixed for 10 minutes via jet mixer valve V2 and pump P2.
     * **Mix 75 (75% GKV + 25% H2O):** 45 m³ GKV + 15 m³ water. Mixed for 15 minutes via jet mixer valve V2 and pump P2.
   * **Storage & Transfer (T3 & T4):**
     * Pump P3 transfers the finished batch from T2 into T3 (Mix 50) or T4 (Mix 75) based on the target recipe.
     * **Recirculation & Extraction:**
       * T3 and T4 supply the deicing vehicles ("Elephant" / "Eisbären") via pumps P4 / P5 and extraction valves (V6 / V9).
       * **Interlock Rule:** When no fluid extraction is taking place, the system continuously recirculates the fluid in T3 (via V5) and T4 (via V8) to maintain homogeneous fluid quality. Recirculation and extraction are strictly mutually exclusive.

   ### Control Implementation (`CtrlDeicingPlant`)
   The Function Block `CtrlDeicingPlant` manages:
   * **Level Control & Inflow Timing:** Calculates fill times based on fixed flow rates (P2 / V1 / V3 = 35 m³/h) to meter exact ratios of GKV and H2O.
   * **Interlock Safety Logic:** Ensures mutually exclusive valve operations (V1 vs. V2, V4 vs. V7, recirculation vs. discharge).
   * **Simulation Integration:** Integrates with `PPS.SimDeicingPlant` using a time-lapse factor (`TLap = 60`) for real-time visualization and testing.
