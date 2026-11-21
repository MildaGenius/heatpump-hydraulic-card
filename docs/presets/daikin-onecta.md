# Daikin Onecta

Preset `daikin_onecta`. Back to the [README](../../README.md#supported-heat-pumps).

## Setup

Install and configure the Home Assistant integration [Daikin Onecta](https://github.com/jwillemsen/daikin_onecta), then add the card:

```yaml
type: custom:heatpump-hydraulic-card
preset: daikin_onecta
```

The preset finds entities through the integration's stable translation keys, so entity IDs and Home Assistant's language do not matter. If more than one Onecta device is present, select the correct device with the card's `device` option.

The preset maps:

- the Altherma leaving-water temperature as heat-pump flow and circuit flow, room temperature, tank temperature as hot-water top and outdoor temperature
- live electrical consumption and thermal output, which can be used for an instantaneous COP tile
- the climate entity's selected heat/cool mode; electrical consumption, not the selected mode, decides whether the unit is running
- today's heating electrical consumption as `energy_today`, plus heating and cooling electrical-energy counters when Onecta exposes them for the device
- Onecta's error, warning and caution binary sensors to the card's attention indicator

The optional `cop_live` tile calculates instantaneous COP from current thermal output ÷ current electrical consumption. This is not a monthly or yearly COP. The integration defines monthly and yearly thermal-output entities, but Onecta does not provide them for every device; without matching thermal-output and electrical-consumption counters for the same period, the card cannot calculate a period COP.

The card can infer a hot-water charge when the flow temperature is above the tank temperature. Onecta does not expose a documented diverter-valve or dedicated hot-water charging state for this mapping, so this inference is not a substitute for an actual valve sensor.

## Not mapped

- **Compressor state, defrost, diverter valve, and water flow rate:** the integration does not expose dedicated entities for these signals. Live electrical power is used as the run signal; there is no dedicated defrost animation.
- **Heat-pump and circuit return temperature:** no corresponding entity is defined by the integration. The leaving-water sensor is mapped only as flow, not as return.
- **Buffer sensors; per-circuit return, pump and operating state; DHW target and charge state; backup heater; source-loop temperatures:** no stable integration entities are defined for these hydraulic parts.
- **Produced-energy counters:** Onecta may expose monthly or yearly thermal output for some devices, but not all. Without those counters, only the live COP from current power sensors is available; lifetime heat totals are not defined by the integration.

This preset is built from the integration source; it has not yet been confirmed on a real installation. Daikin Onecta uses cloud polling, so card values follow the integration's update interval.
