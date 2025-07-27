---
layout: project
title: DIY cold mist humidifier
date: 1404-05-05
description: diy cost effective cold mist humidifier that can plug onto any container
tags:
  - electronics
  - embedded
  - programming
  - motor_control
---

# DIY cold mist humidifier
![](Pasted%20image%2020250727222152.png)
**Modular Cold Mist Humidifier Project**
Cold mist humidifiers can be expensive, but we can make our own since cold mist modules are available at an affordable price. However, these modules are tricky to adjust in water, as the water level above the module needs to be maintained at a specific height for proper operation.

### Problem
If the module is stationary:

- The water level will eventually drop as the humidifier operates.
- We would have to manually refill the container to maintain the required water level above the module.

### Proposed Solution

Create a device that:

1. Measures the water level above the mist module.
2. Uses a motor to push the module deeper into the water when the level drops below the required height.
3. Automatically maintains the correct water level without manual refilling.

### Requirements
- **Cold mist module:** The main component generating mist.
- **Water level sensor:** To detect and measure the water level above the module.
- **Microcontroller:** To process the sensor data and control the motor.
- **Motor with adjustable mechanism:** To lower the module as needed based on the water level.
- **Power supply:** For the microcontroller and motor.

### Working Principle
1. The water level sensor continuously monitors the water above the module.
2. If the water level drops below the required height, the microcontroller triggers the motor.
3. The motor lowers the module into the water until the sensor detects the correct water level.
4. The system ensures consistent mist output and eliminates the need for frequent manual refilling.

### Benefits
- Cost-effective compared to commercial humidifiers.
- Modular design allows customization and upgrades.
- Automated operation reduces manual effort.

This project combines practical engineering and automation to solve a common issue with DIY cold mist humidifiers.