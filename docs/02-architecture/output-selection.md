# Four outputs for industrial lamps and andon

## Recommendation

Use four protected 24 V DC high-side sourcing outputs for compatible common-negative LED andon channels. A typical allocation is red, amber, green and buzzer. Actual tower wiring and alarm conventions must be confirmed; do not assume every andon uses this arrangement.

Initial target: 0.5 A/channel continuous, 2 A total simultaneous, at a declared ambient temperature to be established. At nominal 24 V this is 12 W per channel. This target is not a component rating, a released product claim or a guarantee of driving a 12 W lamp: startup current, capacitive inrush and thermal behavior govern compatibility.

| Option | Appropriate use | Limitations |
|---|---|---|
| Protected high-side electronic output | Common-negative 24 V LED lamps, frequent switching, diagnostics | Shared reference; leakage and inrush compatibility; not dry contact |
| Low-side electronic output | Common-positive loads designed for sinking inputs | Different wiring convention; not interchangeable with high-side output |
| Mechanical relay | Dry contact, external voltage domain, compatible isolated command input | Size, coil power, finite electrical life, contact/inrush rating and switching wear |
| External interface relay | Occasional dry-contact requirement while keeping main PCB compact | Additional DIN-rail component; validate coil suppression and output driver |

No mains-voltage loads are included. A 12 V load must not be connected directly to the proposed 24 V output. Supporting a separate 12 V field supply requires explicit supply-domain and diagnostic validation.

## Candidate implementation to evaluate

Two dual-channel protected high-side switches such as the TI TPS272C45 family are an architectural candidate, not an approved BOM selection. Evaluate operating voltage, transient protection, current-limit tolerance, short-circuit recovery, inductive clamp energy, thermal coupling, leakage, open-load diagnostic conditions and sourcing availability. Manufacturer headline current is not the board's usable continuous rating.

Provide hardware-default OFF, per-channel diagnostics where supported, connectors sized for aggregate return current, protected output supply, visible state indicators and measurable test points. Diagnostic leakage must not cause LED ghost illumination. Confirm compatibility with the andon's common connection and logic-input versus power-input terminals.

For a tower with a built-in controller or dry-contact command inputs, use the appropriate interface rather than directly applying 24 V to its command terminals.

## Bench acceptance before release

Test all four channels continuously at worst-case supply/ambient; cold lamp inrush; simultaneous startup with LTE transmit; short circuit to return; open load; reset/brownout/watchdog; commanded OFF leakage; inductive load turn-off if supported; repeated faults; communication loss and restoration. Record waveforms, temperatures and fault reporting. Do not validate by component datasheet ratings alone.

## Primary references

- TI protected high-side switch overview: https://www.ti.com/product-category/power-management/high-side-switches-controllers/switches/overview.html
- TI TPS272C45 datasheet: https://www.ti.com/mx/lit/gpn/tps272c45
- Espressif Ethernet programming guide: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/network/esp_eth.html
- SIMCom technical files (select exact modem variant): https://www.simcom.com/technical_files.html

Consult the exact current component revision before schematic release.
