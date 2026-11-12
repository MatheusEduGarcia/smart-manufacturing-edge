# Baseline engineering issues and required corrections

Audit baseline: 21 September 2026, KiCad 10.0.5. This report accompanies an unfinished prototype design and must not be read as an approval to manufacture.

## A. Critical power-stage issue: U10 / 4 V rail

The current schematic and PCB both connect U10 pin 2 to OUT_FEED and pin 3 directly to +4V. The XL1509 pinout identifies pin 2 as SW and pin 3 as FB. The OUT_4V net contains only L2 pin 1 and D3 cathode, with no U10 SW connection.

| Pin/node | Current connection | Required design review |
|---|---|---|
| U10.1 VIN | +12V | Verify input decoupling/protection |
| U10.2 SW | OUT_FEED (R29/R30/C17 feedback network) | Connect the switching output to the inductor/diode switching node, after full circuit review |
| U10.3 FB | +4V directly | Connect to the feedback divider for the adjustable regulator |
| L2.1 / D3.K | OUT_4V | Must be driven by the switching stage |

Correct the schematic and PCB routing, including the switching loop. Specify the exact adjustable U10 variant and fixed-5-V U3 variant rather than ordering a generic XL1509. The existing 2.26 kohm/1 kohm divider would target approximately 4.01 V in the intended adjustable topology. Validate compensation and transients; no repair to this stage has yet been implemented. Reference: [XLSEMI XL1509 datasheet](https://www.xlsemi.com/datasheet/XL1509-EN.pdf), also included in this folder.

## B. Components and land patterns

- **U3/U10:** existing pads are 0.50 x 0.25 mm with 1.27 mm pitch; compare the complete land pattern to the selected package. Current footprint origins are not necessarily package centers, which affects placement data.
- **C3/C12/C14/C44:** the footprint name suggests a radial component, but the actual pads are SMD, approximately 3.81 x 1.10 mm and 9.50 mm center-to-center. Select components from their real land patterns and dimensions, not the legacy footprint name.
- **C3 and C44:** imported purchasing data was inconsistent with intended capacitance. Conflicting part numbers were removed in the review copy. Exact voltage, ESR, capacitance, package height and MPN remain open.
- **C1/C4/C8:** values are synchronized to the PCB's 22uF/16V; this is documentation consistency, not proof of regulator stability. Validate DC-bias derating, ESR and transient performance.
- **VR1 / LM1117:** output capacitance/ESR and thermal current budget need validation against the exact selected regulator. [TI LM1117 datasheet](https://www.ti.com/lit/ds/symlink/lm1117.pdf).
- **L3/L4:** marked 68uH/2A but assigned a small LQH2MCN_02-family footprint. The stated current is inconsistent with that family. Review actual branch current, DCR and filtering intent. L3 and FB1 connect the same two rails. [Murata family information](https://www.murata.com/products/inductor/power/overview/lineup/pi1).
- **FB1:** specify impedance at frequency, rated current, DCR and exact part.
- **U16:** verify exact ESP32 module land pattern, center pad marked 1.1 with no net, and pin 32 tied to GND versus the module's NC designation. Verify antenna clearance and stencil strategy. [Espressif ESP32-WROOM-32 documentation](https://documentation.espressif.com/esp32-wroom-32_datasheet_en.html).
- **All remaining items:** imported MPNs and descriptions are not approved. Validate diodes/inductors, current ratings, connector pinout, isolation and assembly method. A recovered footprint or STEP model is not a manufacturer-approved land pattern.

## C. DRC and ERC baseline

| Check after preparatory cleanup | Result |
|---|---:|
| PCB DRC errors | 6 |
| PCB DRC warnings | 178 |
| Unconnected PCB items | 0 |
| Schematic ERC errors | 40 |
| Schematic ERC warnings | 1,500 |
| Native schematic/PCB parity warnings | 398 |
| Schematic components / PCB components | 223 / 223 |
| Connected PCB pads compared topologically | 740 |
| Topological mismatches among those connected pads | 0 |

The six PCB errors are four U23 pad clearances of 0.1843 mm against a 0.2000 mm rule, and two U16 copper-to-edge clearances of 0.2940 mm against a 0.3000 mm rule. Correct or explicitly justify local rules after selecting the fabrication process. Do not relax global rules just to obtain a clean report.

Warnings include 50 solder-mask bridges, four padstack warnings, 121 silkscreen issues and three dangling track/via items. Review mask, paste, polarity labels and solderability before assembly.

The 40 ERC errors comprise 26 unresolved text variables, ten power pins without recognized drivers, two pin-type conflicts and two unconnected J13 shield pins. They are not equivalent to 40 electrical shorts. The 1,492 off-grid endpoint warnings originate from import geometry; do not snap the entire schematic blindly. Check contradictory no-connect markers and residual dangling endpoints. Native netlist export still reports an annotation warning.

The 398 parity warnings comprise 199 field mismatches and 199 net-name mismatches. Independent connectivity comparison matched connected-pad groups after annotation repairs; normalize metadata carefully. There are 39 additional schematic pins classified separately from connected PCB pads. Netlist agreement does not detect a circuit error present in both files, such as U10.

Rerun KiCad checks after all engineering changes.

## D. Preparatory changes already made

- Corrected internal instance references for J22, J23 and C44; C44 had retained the C14 instance reference and duplicate pin UUIDs.
- Closed two 0.003 mm schematic wire gaps at C44 so it now exports between +4V and GND.
- Synchronized C1/C4/C8 values with PCB and removed contradictory purchasing fields for selected capacitors.
- Recovered four local symbol libraries, 223 per-reference footprints and relative library tables; retained all 61 STEP model files.
- Refilled PCB zones. Pad positions and pad nets were checked unchanged across all 783 pads during library recovery.
- Prepared 11 Gerbers, two Excellon files, IPC-D-356, placement files and reference drawings. The baseline drill output contains 504 plated holes including four slots, plus two unplated holes.

The review copy only adds English documentation; it does not introduce an electrical repair beyond the preparatory changes above. CAM remains baseline reference material. Regenerate all manufacturing outputs after design correction.

## E. Ethernet and interfaces

The hardware includes LAN8720A/HR911105A Ethernet. GPIO17 is associated with the external PHY clock route. Review reset and strap circuitry; the PHYAD0 default is 0 when low. Verify the firmware clock mode and address by actual link tests. [Microchip LAN8720A datasheet](https://ww1.microchip.com/downloads/en/DeviceDoc/00002165B.pdf).

Confirm analog input ranges, input protection, relay load ratings, RS485 termination, USB/boot behavior, PHY supply filtering, shield/ESD strategy and thermal margins. No bench, EMC, signal-integrity or environmental qualification testing is represented by this package.
