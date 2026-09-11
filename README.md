# 100MW-PV-Power-Plant-case-study

# TECHNICAL REPORT

**Project:** 100 MW (AC) / 119.40 MWp (DC) Ground-Mounted Bifacial Solar PV Plant  
**Location:** Eastern Province, Sri Lanka  

---

### Referenced Documents
* **GD-PL-001:** Panel Layout
* **GD-TL-001:** Transformer Layout
* **GD-SLD-001:** SLD, Transformer Units 1–4
* **GD-SLD-002:** SLD, Transformer Units 5–29

---

## 1. Plant System Architecture

### Plant Architecture & DC/AC Capacity Flow
```
[168,168 PV Modules: 710 Wp] ---> [286 Inverters: 350 kW]
                                         │
    ┌────────────────────────────────────┴────────────────────────────────────┐
    │                                                                         │
    ▼                                                                         ▼
[Clusters #1 to #4 (4 Units)]                             [Clusters #5 to #29 (25 Units)]
• 9 Inverters per cluster (3,150 kW)                      • 10 Inverters per cluster (3,500 kW)
• Split LV: 5 Inv (LV1) + 4 Inv (LV2)                     • Split LV: 5 Inv (LV1) + 5 Inv (LV2)
• 3,750 kVA, 0.8-0.8/33 kV Transformer                    • 4,200 kVA, 0.8-0.8/33 kV Transformer
    │                                                                         │
    └────────────────────────────────────┬────────────────────────────────────┘
                                         │
                                         ▼
                          [33 kV Collector Grid / Switchgear]
```
> *Note: Each cluster has 1 Transformer.*

The plant follows a highly modular, string-inverter-based centralized cluster architecture designed to aggregate at 33 kV for utility export:

* **DC Generation:** 168,168 Bifacial PV modules (710 Wp each) mounted in $2\times14$ portrait tables at a 7° fixed tilt.
* **String Aggregation:** Modules are wired into strings of 28. Every 21 strings feed directly into a single 350 kW string inverter (no separate DC combiner boxes used).
* **Inversion:** 286 string inverters (350 kW, 12-MPPT variant) convert the 1,500 V DC string outputs into 800 V AC (3-phase, 3-wire).
* **LV AC Aggregation:** Inverters are grouped into 29 discrete "clusters." The 800 V AC outputs are gathered via $3\text{C}\times300\text{ mm}^2$ armored cables into central 3,200 A-frame Air Circuit Breakers (ACBs).
* **MV Step-Up:**
  * Clusters #1 to #4 use 3,750 kVA ($0.8/33\text{ kV}$) transformers (9 inverters each).
  * Clusters #5 to #29 use 4,200 kVA ($0.8/33\text{ kV}$) transformers (10 inverters each).
* **Collector Network:** The 33 kV outputs route through MV switchgear to the main 100 MW Collector Substation.

---

## 2. System Capacity Verification

### 2.1 Total PV Module Count
* **Theory:** The total module inventory must perfectly match the product of the array's electrical topology (inverters, strings, and modules).
* **Equation:**  
  $$\text{Total Modules} = N_{\text{inverters}} \times N_{\text{strings/inv}} \times N_{\text{modules/string}}$$
* **Why this equation:** Physically verifies the bill of materials against the electrical single-line diagram design.
* **Substitution:**  
  $$\text{Total Modules} = 286 \times 21 \times 28$$
* **Calculate:** **168,168 modules**

### 2.2 Total Plant DC Peak Capacity
* **Theory:** Peak DC capacity is the total number of modules multiplied by the Standard Test Condition (STC) peak wattage.
* **Equation:**  
  $$P_{DC} = \text{Total Modules} \times P_{max,STC}$$
* **Why this equation:** Defines the plant's absolute maximum DC power generation rating for regulatory and economic sizing.
* **Substitution:**  
  $$P_{DC} = 168,168 \times 710\text{ Wp}$$
* **Calculate:** **119,399,280 Wp = 119.399 MWp**

### 2.3 Total Plant Nominal AC Capacity
* **Theory:** Nominal AC capacity is the sum of the standard active power output of all string inverters.
* **Equation:**  
  $$P_{AC} = N_{\text{inverters}} \times P_{nom,inv}$$
* **Why this equation:** Establishes the plant's maximum real power export limit to the grid.
* **Substitution:**  
  $$P_{AC} = 286 \times 350\text{ kW}$$
* **Calculate:** **100,100 kW = 100.10 MWac**

### 2.4 DC/AC Overplanting Ratio
* **Theory:** Compares the installed DC capacity to the inverter's AC limit. A ratio $> 1.0$ guarantees the inverters run at full capacity longer during the day despite real-world losses.
* **Equation:**  
  $$\text{Ratio} = \frac{P_{DC}}{P_{AC}}$$
* **Why this equation:** Standard metric used to evaluate inverter clipping vs. levelized cost of energy (LCOE) optimization.
* **Substitution:**  
  $$\text{Ratio} = \frac{119.399\text{ MW}}{100.10\text{ MW}}$$
* **Calculate:** **1.193** *(Optimal for Sri Lanka's irradiation)*

---

## 3. PV String Sizing & MPPT Configuration (IEC 62548)

**Assumed Environment (Eastern Province):**  
* $T_{amb,min} = 15^\circ\text{C}$  
* $T_{cell,max} = 70^\circ\text{C}$

### 3.1 Maximum String Open-Circuit Voltage ($V_{oc}$)
* **Theory:** Photovoltaic voltage rises as temperatures drop. The highest voltage occurs at dawn on the coldest day of the year under open-circuit conditions.
* **Equation:**  
  $$V_{oc,max} = N \times V_{oc,STC} \times \left[1 + \left(\frac{\beta_{voc}}{100}\right) \times (T_{min} - 25^\circ\text{C})\right]$$
* **Why this equation:** IEC 62548 mandates that this cold-weather maximum voltage must never exceed the system insulation limit (1500 V DC) to prevent fires.
* **Substitution:**  
  $$V_{oc,max} = 28 \times 49.0 \times \left[1 + \left(\frac{-0.24}{100}\right) \times (15 - 25)\right]$$
* **Calculate:** **1404.93 V** $(< 1500\text{ V})$

### 3.2 Minimum String MPPT Voltage ($V_{mpp}$)
* **Theory:** PV voltage drops as the cell heats up. The operating voltage at maximum heat must stay within the inverter's MPPT window.
* **Equation:**  
  $$V_{mpp,min} = N \times V_{mpp,STC} \times \left[1 + \left(\frac{\gamma_{pmp}}{100}\right) \times (T_{cell,max} - 25^\circ\text{C})\right]$$
* **Why this equation:** Ensures the inverters will not derate or shut down due to under-voltage during hot summer afternoons.
* **Substitution:**  
  $$V_{mpp,min} = 28 \times 40.9 \times \left[1 + \left(\frac{-0.29}{100}\right) \times (70 - 25)\right]$$
* **Calculate:** **995.7 V** *(Fits safely within inverter's 860–1300 V window)*

### 3.3 MPPT Channel Current Loading
* **Theory:** Bifacial modules generate extra current from rear reflections. Paralleling two strings into one MPPT channel sums their currents.
* **Equation:**  
  $$I_{mpp,MPPT} = 2 \times [I_{mpp,STC} \times (1 + \text{Bifacial Gain})]$$
* **Why this equation:** Verifies the current does not exceed the inverter hardware's physical input limit (40 A), which would cause clipping.
* **Substitution:**  
  $$I_{mpp,MPPT} = 2 \times [17.36\text{ A} \times (1 + 0.10)]$$
* **Calculate:** **38.20 A**

---

## 4. DC Cable Sizing & Verification

### 4.1 DC Cable Ampacity
* **Theory:** DC strings must handle the maximum possible short-circuit current safely, including bifacial gains and standard regulatory safety margins.
* **Equation:**  
  $$I_{design} = 1.25 \times [I_{SC,STC} \times (1 + \text{Bifacial Gain})]$$
* **Why this equation:** IEC 62548 requires cables and fuses to be rated for a continuous 1.25 multiplier over maximum $I_{sc}$.
* **Substitution:**  
  $$I_{design} = 1.25 \times [18.40\text{ A} \times 1.10]$$
* **Calculate:** **25.30 A** *(Compliant. The drawn $1\text{C}\times4\text{ mm}^2$ cable carries $\approx32\text{ A}$ derated, and the module max fuse is 35 A)*

### 4.2 DC Cable Voltage Drop
* **Theory:** Long cable runs cause resistive voltage drops, losing generated power as heat.
* **Equation:**  
  $$\%\Delta V = \frac{I_{mpp,STC} \times R \times L_{loop}}{V_{mpp,string}} \times 100$$
* **Why this equation:** Standard Ohm's law check to ensure DC losses are kept below the industry standard limit of 2.0%.
* **Substitution:**  
  $$\%\Delta V = \frac{17.36\text{ A} \times 5.88\ \Omega/\text{km} \times 0.160\text{ km}}{1145.2\text{ V}} \times 100 \quad \text{(Assuming 80 m average run)}$$
* **Calculate:** **1.43%** *(Compliant, $< 2.0\%$)*

---

## 5. LV AC System: Breakers & Cables

### 5.1 Inverter Output Current
* **Theory:** Apparent power (kVA) dictates the total line current flowing through the AC system at maximum thermal limits.
* **Equation:**  
  $$I_{max} = \frac{S_{max}}{\sqrt{3} \times V_{LL}}$$
* **Why this equation:** Determines the baseline current required to size AC cables, MCCBs, and busbars.
* **Substitution:**  
  $$I_{max} = \frac{352,000\text{ VA}}{\sqrt{3} \times 800\text{ V}}$$
* **Calculate:** **254.03 A**

### 5.2 MCCB Trip Setting
* **Theory:** Circuit breakers must carry continuous loads without nuisance tripping while protecting cables from sustained overloads.
* **Equation:**  
  $$I_{trip} \ge 1.25 \times I_{max}$$
* **Why this equation:** Code mandates a 25% safety margin over continuous load to account for thermal harmonics and transient fluctuations.
* **Substitution:**  
  $$I_{trip} = 1.25 \times 254.03\text{ A}$$
* **Calculate:** **317.54 A** *(The drawn 400 A MCCB is Compliant)*

### 5.3 LV Busbar and ACB Sizing
* **Theory:** The busbar must safely carry the summed output of all connected inverters.
* **Equation:**  
  $$I_{bus} = N_{inv,per\ bus} \times I_{max}$$
* **Why this equation:** Defines the peak current entering the Main Air Circuit Breaker (ACB).
* **Substitution (5-Inverter Bus):**  
  $$I_{bus} = 5 \times 254.03\text{ A}$$
* **Calculate:** **1,270.15 A** *(The drawn 2000 A Busbar and 3,200 A ACB are Compliant)*

---

## 6. Transformer Sizing & Grid Margin (IEC 60076)

### 6.1 Peak Transformer Loading Ratio
* **Theory:** The cluster transformer must handle the combined peak apparent power of its connected inverters without exceeding its rating.
* **Equation:**  
  $$\text{Loading Ratio } (\%) = \left(\frac{N_{inv} \times S_{max,inv}}{S_{transformer}}\right) \times 100$$
* **Why this equation:** Ensures the transformer operates safely below 100% capacity to allow for tropical ambient temperature derating.
* **Substitution (4,200 kVA Clusters):**  
  $$\text{Ratio} = \left(\frac{10 \times 352\text{ kVA}}{4200\text{ kVA}}\right) \times 100$$
* **Calculate:** **83.81%** *(Compliant, provides healthy $\sim 16\%$ margin)*

### 6.2 Reactive Power Support (CEB Grid Code)
* **Theory:** Transformers must pass enough apparent power to support grid voltage regulation (reactive power) while pushing full real power (active power).
* **Equation:**  
  $$S_{req} = \frac{P_{active}}{\text{Power Factor}}$$
* **Why this equation:** CEB mandates $\pm0.95$ power factor support at the Point of Connection.
* **Substitution (4,200 kVA Clusters):**  
  $$S_{req} = \frac{3500\text{ kW}}{0.95}$$
* **Calculate:** **3,684 kVA** $(< 4200\text{ kVA})$

---

## 7. CRITICAL THERMAL RISK: LV Busduct Analysis (Tray vs Buried)

The SLD specifies $4\text{R}\times1\text{C}\times630\text{ SQ.MM AL/XLPE/SWA/PVC}$ between the combined LV busbars and the transformer.

$$\text{Required Current} = \frac{4,200,000}{\sqrt{3} \times 800} = 3,031\text{ A}$$

### 7.1 Scenario A: Ventilated Cable Tray (In Air)
* **Theory:** Cables grouped on a tray mutually heat each other and are limited by the hot ambient air.
* **Equation:**  
  $$I_{bundle} = N \times (I_{base} \times C_{ambient} \times C_{grouping})$$
* **Why this equation:** IEC 60364-5-52 thermal derating calculation to prevent insulation melting.
* **Substitution:**  
  $$I_{bundle} = 4 \times (800\text{ A} \times 0.91 \times 0.85)$$
* **Calculate:** **2,475 A (FAILS.** Falls short of 3,031 A).

### 7.2 Scenario B: Buried Duct Bank (In Ground)
* **Theory:** Burying cables inside ducts severely traps heat, causing drastic capacity reductions.
* **Equation:**  
  $$I_{bundle} = N \times (I_{base,soil} \times C_{soil-temp} \times C_{duct-grouping})$$
* **Why this equation:** Stricter IEC buried derating standards.
* **Substitution:**  
  $$I_{bundle} = 4 \times (500\text{ A} \times 0.88 \times 0.65)$$
* **Calculate:** **1,144 A (CATASTROPHIC FAILURE.** Cable insulation will rapidly melt).

> **Mandated Fix:** Discard the 4x Aluminum design. Use 6x Aluminum, 4x Copper, or a rigid cast-resin busbar trunking system (IP68, 3,200 A).

---

## 8. Medium Voltage (33 kV) Fault Withstand

### 8.1 MV Cable Short-Circuit Adiabatic Check
* **Theory:** During a grid fault, cables must absorb massive thermal energy without the XLPE insulation degrading before the breaker trips.
* **Equation:**  
  $$S_{min} = \frac{I_{sc} \times \sqrt{t}}{k}$$
* **Why this equation:** Determines the absolute minimum cable cross-section ($S$) required to survive the fault.
* **Substitution:**  
  $$S_{min} = \frac{25,000\text{ A} \times \sqrt{1.0\text{ s}}}{94} \quad \text{(Standard CEB 25 kA fault, Al constant 94)}$$
* **Calculate:** **265.95 mm²**
* **Verdict:** The drawn $1\text{C}\times95\text{ mm}^2$ Al cable fails. It can only withstand 8.93 kA and will explode during a severe grid fault. Upgrade to $3\text{C}\times185\text{ mm}^2$ Cu or $300\text{ mm}^2$ Al.

---

## 9. Land Use & Shading Geometry

### 9.1 Inter-Row Shadow Clearance
* **Theory:** The physical space between PV rows must be longer than the shadow cast by the tables during peak hours (9 AM – 3 PM) at the winter solstice.
* **Equation:**  
  $$L_{shadow} = \frac{L_{table} \times \sin(\text{tilt})}{\tan(\text{solar altitude})}$$
* **Why this equation:** Geometric verification to guarantee no module-level self-shading, which severely limits MPPT yield.
* **Substitution:**  
  $$L_{shadow} = \frac{(2 \times 2.384\text{ m}) \times \sin(7^\circ)}{\tan(30^\circ)} \quad \text{(Assuming portrait mounting and } 30^\circ \text{ morning sun angle)}$$
* **Calculate:** **1.01 m**
* **Verdict:** $1.01\text{ m} \le 1.5\text{ m}$ drawn clearance.

---

## 10. Rectification Action Plan (EPC Directives)

1. **33 kV Protection Hazard:** Remove the "33kV DO FUSE". Replace with 33 kV Vacuum Circuit Breakers (VCBs) or Motorized RMUs linked to transformer alarm trips.
2. **Surge Arresters:** Correct the "Type II" SPD mislabeling to IEC 60099-4 Station Class ZnO Arresters.
3. **Cable Armor:** Change the 1C AC cable armor from SWA (Steel) to AWA (Aluminum) to prevent magnetic overheating.
4. **Cable Sizing:** Upsize the 33 kV cable from $95\text{ mm}^2$ to $3\text{C}\times185\text{ mm}^2$ Cu to survive 25 kA faults. Redesign the LV busduct (Section 7).
5. **Efficiency:** Downgrade the 4-Pole busbars and ACBs to 3-Pole (3P) to eliminate wasted copper on the 3-wire 800 V output.
6. **Data Updates:** Input exact Latitude/Longitude coordinates into the title blocks, and recalculate the 58.52 A CT template for the specific $3.75 / 4.2\text{ MVA}$ loads.
