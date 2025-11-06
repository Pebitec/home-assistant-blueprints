# Home Assistant Blueprints

Smart blueprints for Home Assistant automation.

## Blueprints

### Multi-Zone Smart HVAC Control (v1.3.0)

A comprehensive blueprint to control up to three independent HVAC zones based on a sophisticated set of rules for temperature, energy prices, and solar production.

This blueprint is designed for maximum efficiency, allowing you to heat your home using the cheapest and greenest energy available.

#### Core Features

* **Multi-Zone Control**: Manages three zones independently.

* **Zones 1 & 2**: Standard, full-featured logic.

* **Zone 3**: A simpler, custom logic mode ideal for secondary areas (e.g., garages, basements) based on a separate solar surplus threshold.

* **Advanced Energy Logic**:

* **Solar Surplus**: Heats to a high target when solar production exceeds your home's base consumption.

* **Grid Export**: Heats to a high target when you are actively exporting power back to the grid.

* **Low Price**: Heats to a medium target when electricity prices are below your threshold.

* **Poor Conditions**: Maintains a low, base temperature when energy is expensive.

* **Vacation Mode**:

* Set all zones to a low "night" temperature.

* Includes a "Solar Boost" feature to heat the house to its *high* target if massive solar production is detected (e.g., > 5000 W), using free energy even while you're away.

* **Night Mode**: Set specific, lower temperatures for Zones 1 & 2 during the night.

* **Smart Fan Control**: Uses a "Normal" fan mode for regular heating and automatically engages a "Boost" fan mode when the temperature difference is large, heating the room faster.

* **Debug Notification**: Creates a persistent notification in Home Assistant showing the current state, targets, and decisions for each zone, making it easy to see *why* the automation is doing what it's doing.

#### Required Sensors & Entities

* **Global:**

* Solar Production Sensor (W)

* Net Grid Consumption Sensor (W)

* Electricity Price Sensor (with `current_price` attribute)

* (Optional) Vacation Mode `input_boolean` helper

* **For Each Zone:**

* Temperature Sensor

* HVAC Climate Entity

* (Optional for Z1/Z2) Heat Pump Consumption Sensor (W)

#### Installation

1. In Home Assistant, go to **Settings** → **Blueprints**.

2. Click **Import Blueprint** in the bottom-right corner.

3. Paste the URL of this blueprint's file: `https://github.com/Pebitec/home-assistant-blueprints/blob/main/climate/smart_hvac_temperature_energy_price_control.yaml`

### Ping Dead Z-Wave Devices (v1.0.6)

A utility blueprint that automatically pings dead, unavailable, or unknown Z-Wave JS devices to try and bring them back online.

#### Core Features

* **Automatic Recovery**: Runs automatically when a device's status changes to 'dead'.

* **Safe & Efficient**: Only runs when dead devices are detected (count > 0).

* **Self-Contained Setup**: The blueprint description includes the `template sensor` code required for it to work.

* **Logger**: Creates a system log entry when it runs, so you know which devices it tried to ping.

#### Required Sensors & Entities

* **Template Sensor**: Requires the `sensor.dead_zwave_devices` template sensor. The code for this sensor is included in the blueprint's description for easy setup.

#### Installation

1. In Home Assistant, go to **Settings** → **Blueprints**.

2. Click **Import Blueprint** in the bottom-right corner.

3. Paste the URL of this blueprint's file: `https://github.com/Pebitec/home-assistant-blueprints/blob/main/automation/zwave_ping_blueprint.yaml` (Note: You'll need to create this file in your repo)

#### Configuration

1. **Create the Template Sensor**: Copy the `template:` code from the blueprint's description into your `configuration.yaml` or `sensors.yaml` file and restart Home Assistant.

2. **Create the Automation**: Create an automation from the blueprint and select your newly created `sensor.dead_zwave_devices`.
