Navigation Sensors
==================

To help track the location of the robot on the field, a device can be used that combines two sensors:
   - accelerometer - track the change in velocity over time; integrate twice to find distance traveled
   - gyroscope - track the rate of turning; integrate over time to calculate the total angle turned

FRC teams typically use one of these two products as navigation sensors:

Pigeon
------

- A small board that connects into the CAN bus
- Made by Cross the Road Electronics
- `Pigeon (Discontinued) Product Page <https://store.ctr-electronics.com/gadgeteer-pigeon-imu/>`_
- `Pigeon 2 Product Page <https://store.ctr-electronics.com/pigeon-2/>`_
- `Software Reference (for all CTRE products) <https://store.ctr-electronics.com/software/>`_

NavX
----

- A larger board that plugs directly into the large expansion port on the RoboRIO
- Made by Kuali Labs
- `NavX Product Page <https://pdocs.kauailabs.com/navx-mxp/>`_
- `NavX Programming Reference <https://pdocs.kauailabs.com/navx-mxp/software/roborio-libraries/java/>`_
