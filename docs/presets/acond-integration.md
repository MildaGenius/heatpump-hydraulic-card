# Acond (Acond Integration)

Preset `acond_integration`. Back to the [README](../../README.md#supported-heat-pumps).

## Setup

Install and configure [Acond Integration](https://github.com/MildaGenius/HA_AcondIntegration), then add the card:

```yaml
type: custom:heatpump-hydraulic-card
preset: acond_integration
```

The preset detects the compressor on the `acond_integration` platform. It supports both the catalog ID `binary_sensor.page49_compressor` and the entity IDs reported by a Home Assistant installation, such as `binary_sensor.acond_heat_pump_compressor`.

The preset maps:

- the compressor binary sensor as the running state
- the heat-pump circulation pump as `primary_pump_state` and the system circulation pump as `hc_pump`
- the outdoor fan binary sensor as fan activity
- the plate heat-exchanger outlet temperature as heat-pump flow
- the domestic-hot-water heating binary sensor as the heating / hot-water route
- outdoor temperature, hot-water temperature and buffer temperature
- the requested buffer temperature, shown as a target on the buffer
- room temperature, `target_domestic_hot_water_temperature` and `desired_room_temperature`

This mapping is based on Acond Integration 1.0.0 and has been checked against a real Home Assistant dashboard. The Acond heat-pump manual identifies the plate-exchanger outlet sensor as P02 in the alarm table (printed page 28), and identifies the outdoor fan and two circulation pumps in the RCM2-1 symbols (printed page 10). During a reported hot-water charge, `circulation_pump` was on while the Acond display showed the heat-pump pump running and the system pump stopped; `circulation_pump_output` was off. During space heating, `circulation_pump_output` was reported on. These observations map the heat-pump pump to `primary_pump_state` and the system pump to `hc_pump`. The card derives heating versus hot-water operation from the compressor and hot-water heating flag; the integration does not provide a separately identified operating-mode entity.

## Not mapped

- **Heat-pump return and heating-circuit flow/return, flow rate, power, heat output, separate operating mode, defrost and error:** the manual identifies a system-return probe as P01, but the supplied entity list has no distinct entity for it; the remaining roles are not identified reliably.
- **Domestic-hot-water recirculation pump, backup heater and energy counters:** the catalog does not establish a DHW recirculation-pump role or define heat-pump energy counters.

The preset deliberately does not infer hydraulic roles from unidentified PLC addresses. Those entities can be added manually in the card configuration when their meaning is known for the installation.
