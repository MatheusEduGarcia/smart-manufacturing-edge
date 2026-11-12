# Requirements and open decisions

## User-confirmed architecture

One Ethernet port connects to OT. Wi-Fi or cellular provides upstream connectivity. Four physical outputs are required for lamps/andon. This device collects machine data and publishes it for smart-manufacturing applications; it is not a general-purpose OT/IT router or safety controller.

## Proposed engineering targets - not released specifications

| Function | Proposed baseline | Required validation |
|---|---|---|
| Supply | Nominal 24 V DC | Operating/transient range, reverse polarity, surge, fuse and thermal budget |
| Digital inputs | 8 isolated 24 V channels | PNP first; NPN compatibility and input thresholds to be explicitly designed, pulse rate TBD |
| Analog inputs | 4 channels: initially 2 x 4-20 mA and 2 x 0-10 V | Sensor survey, accuracy, external ADC, filtering, overvoltage and isolation strategy |
| Outputs | 4 protected positive-switching 24 V outputs | 0.5 A/channel, 2 A aggregate targets; ambient, inrush, leakage, protection and connectors |
| Ethernet | One OT port | PHY/MAC, magnetics, ESD, EMC and network recovery |
| RS-485 | One galvanically isolated interface | Isolated power, selectable termination, bias, TVS and common-mode limits |
| Wireless | Wi-Fi; preserve BLE capability for potential commissioning | Antenna clearance, pairing and coexistence; BLE features not yet committed |
| Cellular | LTE modem with SIM and external antenna | Exact regional SKU, bands/carrier support, interface, current transients and thermal budget |
| Storage | Local telemetry buffer | Retention target, wear, overflow policy and recovery |
| Time | UTC synchronization plus monotonic event ordering | Source, uncertainty and offline behavior; RTC need TBD |
| HMI | External client first | Protocol, network placement and display model TBD |

## Firmware contract

- Modbus TCP client to read machine registers; optionally expose a bounded server map for an HMI. Separate permissions and document byte/word order, scaling, polling limits and timeouts.
- Modbus RTU client over RS-485. Server mode only if a concrete integration needs it.
- MQTT over TLS as the proposed upstream transport over Wi-Fi or LTE. Confirm the broker, schema, credentials and provisioning before implementation.
- Application-level acquisition and publication only. IP forwarding, NAT and transparent bridging disabled in the initial design.
- OPC UA integration initially belongs on the external edge/server. Native embedded client feasibility is a separate work item, not an existing feature.
- Publish device ID, asset ID, channel, value, unit, UTC timestamp, time quality, measurement quality and sequence number. Maintain schema version and configuration provenance.
- Semantic mappings, historical storage, ontology, graph and AI services run externally.
- Output state is controlled by explicit authorized rules/commands with expiry. No direct AI actuation by default.
- Outputs held OFF by hardware during reset/startup; defined OFF default on watchdog or expired command. A different communications-loss indication must be intentionally configured and tested. Not a safety function.
- Provision unique credentials, authenticated firmware updates and local service access. Validate TLS memory usage and reconnect behavior under load.

Open decisions: real load models and inrush; andon common wiring; input sensor list; pulse frequency; analog accuracy/isolation; LTE country/operator; offline retention; enclosure/environment; simultaneous output loading; supply source; output indicator conventions. Do not order components before these electrical dependencies are resolved.
