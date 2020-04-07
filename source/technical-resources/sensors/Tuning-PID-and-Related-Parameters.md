# Tuning PID and Related Parameters

These are very brief notes on tuning the various motor control parameters available on the Talon SRX motor controller. Most are related to closed-loop control, but there are a few knobs for open-loop modes (eg: joystick driving during teleop.) The API calls referenced below are methods of a TalonSRX or WPI_TalonSRX object.

## Open-Loop Parameters

`configOpenloopRamp(secondsFromZeroToFullThrottle)`

We typically want the robot to go from stopped to full speed in something like 0.5 to 2.0 seconds.

## Motion Magic Tuning

* Capture the sensor velocity when driving at full speed using the roboRIO CAN interface for a drive Talon
    * fullspeed = ________ sensor positions/100 ms
* Set feed-forward gain (F) so 100% motor output is calculated when requesting the above full speed
    * F = (100% * 1023) / fullspeed = ________
* The configMotionCruiseVelocity should be less than the full speed, perhaps 75%
    * cruise = 75% * fullspeed = ________
* The configMotionAcceleration value sets a limit on the change in velocity per second. If we want to achieve full speed in half a second, the acceleration is twice the full speed value.
    * secondsFromZeroToFull = ________ (maybe 0.5?)
    * accel = fullspeed / secondsFromZeroToFull = ________
* Set the proportional gain (P) based on an average error in position seen when running with only the above parameters set. Then decide what percent of throttle you want to use to this correct this typical error.
    * typError = ________  sensor positions
    * correction = ________ (maybe 10% = 0.1?)
    * P = (correction * 1023) / typErr = ________
* To test the calculated value of P, use it to drive to some position. While it's still running, back-drive the motor to see if it promptly responds to additional error. If you're able to move the motor further than you'd like, double P until it is responsive enough.
    * P = P * 2^n, where n=0, 1, 2, etc.  = ________
* Set the derivative gain (D) to correct for any overshoot at the end of travel. If overshooting is observed, start with 10 times the proportional gain.
    * D = P * 10 = ________
* If the mechanism being controlled needs to have very precise positioning, you may need to use the integral gain and integral zone to clean up any residual error. If the above calculations are good enough, skip these values. Because integral values accumulate error over time, they can add up quickly and introduce massive throttle changes at surprising times. Start by observing the typical residual error in positioning. Start with an integral zone (range of error values over which those errors are accumulated into the I term) of two times the typical error.
    * typResidual = ________  sensor positions
    * iZone = 2 * typResidual = ________
    * I = 1/100 * P = ________
* If the residual error is not resolved, you can either increase the integral zone or double the integral gain
    * iZone = 3 * typResidual = ________
    * I = I * 2^n, where n=0, 1, 2, etc.  = ________
