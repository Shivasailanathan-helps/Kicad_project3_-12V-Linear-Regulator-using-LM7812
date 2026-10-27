# Kicad_project3_-12V-Linear-Regulator-using-LM7812
This project features the schematic capture and dual-layer PCB layout of a +12V Regulated DC Power Supply, designed in KiCad EDA. The board converts an AC input into a clean, stable +12V DC output suitable for powering sensitive analog/digital electronics and bench prototyping setups.

## Features:
Rectification & Primary Filtering: Full-wave bridge rectifier (D1 to D4) paired with a 1000muF electrolytic capacitor for low-frequency AC ripple suppression.
Linear Voltage Regulation: LM7812 IC (TO-220 package) delivering steady +12V output regulation.
Output Decoupling: Parallel combination of a 470muF electrolytic bulk storage capacitor and a 0.1muF(104) ceramic high-frequency bypass capacitor for transient response and noise filtering.
Visual Status Indication: Onboard status LED D5 with a 1k current-limiting resistor R1.
Dual-Layer Layout Strategy: Utilizes a Top Copper (F.Cu) +12V power pour for component heat sinking alongside a continuous Bottom Copper (B.Cu) Ground (GND) plane for low-impedance return paths and EMI reduction. Surface-mount ground pads are connected directly to the ground plane using plated vias.

## Process:
### Phase 1: Schematic Capture & ERC Verification
1. **Bridge Rectification & Filtering:** Built a full-wave diode bridge (D1-D4) fed by connector $J_1$, filtered by bulk capacitor C1 to smooth AC mains ripple before linear regulation.
2. **Linear Regulation & Decoupling:** Integrated the LM7812 regulator ($U_1$) with parallel output capacitors (C2 for load transient recovery, C3 for high-frequency noise attenuation).
3. **ERC & `PWR_FLAG` Configuration:** Resolved KiCad "undriven power pin" Electrical Rules Check errors by adding `PWR_FLAG` symbols to both the positive rectified rail and Ground node. This tells KiCad's static pin-checking engine that power enters the net via passive components (J1, diodes).
4. **Netlist Junction Repair:** Diagnosed an issue where return pads were assigned an auto-generated net name (`Net-(D3-A)`) rather than `GND`. Located a missing junction dot on the schematic where LM7812 Pin 2 intersected the main return rail. Re-establishing the junction node restored full netlist integrity across all return pads.

---

### Phase 2: PCB Layout & Copper Pour Strategy
1. **Board Boundary Definition:** Defined a closed PCB perimeter on the **`Edge.Cuts`** layer to establish the physical board outline and enable shaded area rendering.
2. **Bottom-Layer Ground Plane (`B.Cu`):** Flooded the bottom copper layer with a solid **`GND`** zone. Through-hole component pins automatically tied into this continuous $0\text{V}$ reference plane via thermal relief spokes.
3. **Top-Layer Power Plane (`F.Cu`):** Poured a **`+12V`** copper zone across the output side of the top layer. This plane acts as a low-impedance power distribution rail while serving as an onboard heat sink for the LM7812 IC.
4. **Via-Stitching for Surface-Mount Pads:** Placed plated vias adjacent to surface-mount (SMD) ground pads (C2, C3, D5), dropping top-layer SMD return paths straight down into the continuous bottom ground plane.

---

### Phase 3: Verification & Inspection
* **3D Visual Audit:** Verified component clearances, mechanical footprint orientations, and solder mask openings in KiCad's 3D Viewer (`Alt + 3`).
* **Net Highlighting:** Conducted net-continuity checks on the `+12V` rail to ensure low-resistance paths from the LM7812 output terminal (`VO`) to the DC terminal block.

## Extras:
### Why the Top Copper (`F.Cu`) Zone Covers Only the Right Half?

The top-layer red copper pour (`F.Cu`) is intentionally restricted to the right half of the board, while the bottom-layer blue ground plane (`B.Cu`) extends across the entire PCB. This design decision is based on three core PCB engineering principles:

#### 1. Power Domain Separation (AC/Raw DC vs. Regulated DC)
* **Left Half (Unregulated / AC Domain):** Contains the AC input connector (J1) and the full-wave bridge rectifier (D1-D4). This region carries alternating current and raw, pulsating DC prior to regulation.
* **Right Half (Regulated DC Domain):** Begins at the output pin (Pin 3) of the LM7812 regulator (U1). 
* **Reason:** The +12V power rail physically does not exist until *after* the linear regulator. Extending a +12V copper plane into the left section would be electrically invalid and risk shorting out distinct AC lines or raw diode nodes.

#### 2. EMI & Noise Isolation
* Diode switching and AC input traces generate localized electromagnetic noise and ripple.
* Restricting the +12V copper pour strictly to the output side prevents AC switching noise from capacitively coupling into the clean, filtered DC output plane.

#### 3. Continuous Bottom Ground Plane (`B.Cu`) Extension
* While the top layer (`F.Cu`) is split into manual traces on the left and a +12V plane on the right, the bottom layer (`B.Cu`) spans the full PCB length.
* This provides a continuous, unbroken 0V ground return path across all circuit stages, minimizing current loop areas, reducing EMI radiation, and acting as a shield for the entire board.

## Outputs:
<img width="1600" height="899" alt="image" src="https://github.com/user-attachments/assets/fc4103ae-675c-4766-98f4-823aa755d46f" />
<img width="1600" height="899" alt="image" src="https://github.com/user-attachments/assets/abef406e-43eb-4605-bfc0-bb3c0250e2ad" />
<img width="1600" height="899" alt="image" src="https://github.com/user-attachments/assets/eb39e43f-83da-4713-8d85-518ba0291ba0" />



