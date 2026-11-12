# Implementation and release plan

1. Confirm actual andon wiring/current/inrush, sensors, pulse rate, analog accuracy, LTE region/operator and environment. Validate proposed 8 DI / 4 AI quantities.
2. Bench-prototype the Ethernet OT + Wi-Fi/LTE application with existing development hardware. Demonstrate Modbus read, timestamp/quality schema, encrypted publication, offline buffer and reconnect behavior. Verify no IP forwarding.
3. Create power and GPIO budgets; select MCU/module, external ADC, I/O expansion, LTE SKU and protected output devices. Verify all four load currents plus modem bursts and isolation requirements.
4. Draw the new hierarchical schematic. Review every selected part against its exact datasheet and footprint. Replace the SIM7600 placeholder. Correct/re-engineer inherited power circuits. Run ERC and independent connection review.
5. Design the new PCB placement and routing, including Ethernet, RF, return paths, switcher loops, analog separation, heat, isolation and antenna clearances. Board dimensions follow engineering placement rather than prior concept images.
6. Run DRC, schematic/PCB parity and library/model checks. Validate a procurement BOM and review DFM/DFA with the supplier. Export fresh CAM/CPL; do not reuse source-project Gerbers.
7. Build a small engineering prototype batch after scope/cost agreement. Bring up power first, then MCU/network, inputs and outputs. Test cellular bursts with maximum output load, brownouts, faults, buffering and upstream failover.
8. Complete enclosure, test procedure, firmware/update process, calibrated input results and interface documentation. Release an immutable manufacturing package only after measured acceptance.

## Supplier request

Quote engineering and prototype assembly for the revised single-Ethernet OT / Wi-Fi+LTE architecture. Include four protected DC outputs instead of eight onboard relays. Treat the two source projects as incomplete references. Itemize hardware engineering, firmware, PCB/assembly, enclosure and validation. Do not fabricate the references directly.

## Deliverables still missing

Merged validated schematic and PCB; approved BOM; measured output/input ratings; firmware; exact modem and antenna selection; mechanical CAD; EMC/environment test plan; new Gerbers/drills/CPL; prototype acceptance report. These are not supplied by the architecture documents.
