Multi-Zone HVAC Control Blueprint for Home Assistant

Version: 1.5.0

This highly customizable Home Assistant blueprint intelligently manages up to three separate HVAC (climate) zones based on real-time room temperatures, electricity prices, solar production, and heat pump consumption. It is designed to maximize comfort while minimizing energy costs by automatically utilizing surplus solar energy and taking advantage of low grid prices.

✨ Features

Intelligent Energy Optimization: Automatically calculates the optimal target temperature by evaluating:

Solar Surplus: Heats the house when you are producing more solar power than you are consuming.

Energy Exporting: Maximizes heating when you are actively sending excess energy back to the grid.

Electricity Prices: Takes advantage of cheap electricity to pre-heat or maintain temperatures.

Three Independent Zones:

Zones 1 & 2: Share global thresholds but maintain separate target states, fan controls, and heat pump consumption tracking.

Zone 3 (Custom Logic): Operates on a dedicated, highly simplified solar-surplus logic with its own temperature settings and independent hysteresis.

Vacation Mode: Drops temperatures to a baseline level while you are away, but intelligently boosts heating if your solar production exceeds a massive threshold (e.g., 5000W), ensuring your home stays fresh and dry using free energy.

Individual Zone Toggles: Enable or disable the automation for each zone independently using an input_boolean helper without having to disable the entire automation.

Custom Fan Controls: Supports standard numeric fan modes (e.g., 1-5) and named fan modes (e.g., low, high, strong), with the ability to input custom strings if your specific HVAC integration requires it.

Anti-Short-Cycling (Hysteresis): Built-in hysteresis prevents your heat pump from turning on and off constantly when hovering around the target temperature.

Night Mode: Automatically lowers the temperature during specified sleeping hours (applies to Zones 1 & 2).

Live Debugging: Generates a live, updating persistent notification in your Home Assistant dashboard detailing the exact conditions, variables, and logic decisions being made every 5 minutes.

📋 Prerequisites & Required Entities

To get the most out of this blueprint, you will need the following entities in your Home Assistant instance:

Global Sensors

Solar Production Sensor (power): Measures total solar generation (e.g., sensor.solar_power).

Net Consumption Sensor (power): Measures what is coming from / going to the grid. Positive = importing, Negative = exporting. (e.g., sensor.grid_power).

Electricity Price Sensor: A sensor that includes a current_price attribute (like the standard Nordpool integration).

Zone-Specific Entities

For each active zone, you need:

A Temperature Sensor for the room.

A Climate Entity (your HVAC/Heat Pump).

(For Zones 1 & 2) A Power Sensor measuring the heat pump's current consumption.

Helpers (Input Booleans)

You will need to create Toggle Helpers (input_boolean) via Settings > Devices & Services > Helpers:

Vacation Mode Toggle (Optional)

Zone 1 Enable Toggle (Optional)

Zone 2 Enable Toggle (Optional)

Zone 3 Enable Toggle (Optional)

⚙️ How It Works (The Logic)

The blueprint runs every 5 minutes (and exactly at Night Mode Start/End times) to evaluate your home's energy state.

Energy Conditions

The automation categorizes your energy situation into four tiers:

Best (Exporting): Your net consumption is negative and below the Energy Export Threshold. (You have lots of free energy).

Good (Solar Surplus): Your total solar production minus your Base Household Consumption is greater than the Solar Production Threshold.

OK (Low Price): Your electricity price is below the Electricity Price Threshold and your heat pump is not currently consuming excessive power.

Poor: None of the above conditions are met.

Target Calculation (Zones 1 & 2)

Best / Good Conditions: Sets the HVAC to the Target Temperature at Good Conditions.

OK Conditions: Sets the HVAC to the midpoint between the Good and Poor temperatures.

Poor Conditions: Sets the HVAC to the Target Temperature at Poor Conditions.

Night Mode / Vacation Mode: Overrides the above to set the HVAC to the designated Night/Base temperatures (unless a massive vacation solar surplus is detected).

Custom Logic (Zone 3)

Zone 3 uses a simplified approach ideal for less critical spaces (like a garage or basement). It looks strictly at your Net Consumption. If you are exporting more than the Zone 3 Surplus Threshold (e.g., -1000W), it sets the "Solar Temperature". Otherwise, it falls back to the "Base Temperature".

🚀 Installation & Setup

Import this blueprint into your Home Assistant instance.

Create a new Automation and select the Multi-Zone HVAC Control blueprint.

Map all your required global sensors (Solar, Net Consumption, Price).

Fill out the configuration for Zone 1. If you only have one zone, you can leave the entities for Zones 2 and 3 blank (or use dummy sensors if your setup complains).

Ensure your Fan Modes perfectly match what your specific Climate integration expects. If the dropdown values do not work, type the exact string your integration uses (e.g., "turbo", "quiet") directly into the dropdown box.

Save the automation.

Troubleshooting

Check your Home Assistant Notifications panel. The blueprint automatically generates a hvac_control_debug notification. This will show you exactly what energy tier the automation selected, the calculated target temperatures, and why it decided to turn the HVAC HEAT, OFF, or IDLE.
