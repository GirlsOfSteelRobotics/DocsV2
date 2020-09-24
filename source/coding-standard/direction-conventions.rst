.. _direction-conventions:


Direction Conventions
=====================


Motor Conventions
-----------------

Motors should be set up such that moving in the "positive direction" should result in a green LED, and be set with a positive speed value
This may require inverting the polarity of the motor controller. This is especially evident for our tank drive set up, since
one side is mechanically inverted.

The "positive direction" is defined such that it causes the mechanism to move in the direction the direction that leads to us scoring or something of similar nature.

Examples Include:

- Driving the robot forwards
- Moving an elevator up to lift a game piece
- Spinning a shooter wheel in the direction that ejects the game piece 

Digital Input Conventions
-------------------------

Any getters in the code or things sent to the dashboard should return :code:`true` when the sensor is tripped. This is 
often the opposite of how the sensor behaves natively, so it must be inverted in the code

Examples:

- :code:`boolean getBallSensor()` should return true when a ball is detected, and false otherwise


Field Coordinate System
-----------------------

The field coordinate system matches the standard mathematical definition (which sucks).

Imagine the field is 
laid out such that the long dimension goes along the x axis, and the short dimension goes along the y axis.
The origin point (0, 0) is located at the top left, while the bottom right is defined as (long dim, -short dim).

.. parsed-literal::
   
   (0, 0)
   -----------------------------------------
   \|                                       |
   \|                                       \|
   \|                                       \|
   \|                                       \|
   \|                                       \|
   \|                                       \|
   ----------------------------------------- (54 ft, -27 ft)

   
Angles are measured from the X-Axis, increasing counter-clockwise.

.. parsed-literal::

   → = 0 degrees
   ↑ = -90 degrees
   ↓ = 90 degrees
   ← = 180 degrees