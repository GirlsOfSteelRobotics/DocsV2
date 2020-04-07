# LIDAR-Lite

LIDAR-Lite is a low-cost distance sensor with a range and accuracy that makes it ideal for use on FRC robots. For around $150, it can range over 40 meters with an accuracy of 2.5 cm. It was created by [Pulsed Light](https://www.pulsedlight3d.com/), now part of [Garmin](https://www.garmin.com/en-US/). There have been three version of the product. The first two were sold by Pulsed Light before the Garmin acquisition. Some useful links:

* A few of our favorite sources for LIDAR-Lite v3 (current model as of January 2018)
    * https://buy.garmin.com/en-US/US/p/557294
    * https://www.sparkfun.com/products/14032
    * https://www.andymark.com/product-p/am-3829.htm
* Documentation on the various versions
    * Garmin [LIDAR Lite v3 Operation Manual and Technical Specifications](https://static.garmin.com/pumac/LIDAR_Lite_v3_Operation_Manual_and_Technical_Specifications.pdf)
    * AndyMark copy of [Operation Manual and Technical Specifications](http://files.andymark.com/PDFs/pli-06-instruction.pdf)
    * [LIDAR Lite v2 Manual](https://github.com/PulsedLight3D/LIDAR-Lite-Documentation/blob/master/Docs/LIDAR-Lite-v2-Docs.pdf) on GitHub
    * [LIDAR Lite v1 Manual](https://github.com/PulsedLight3D/LIDAR-Lite-Documentation/blob/master/Docs/LIDAR-Lite-v1-docs.pdf) on GitHub
    * [Garmin software library](https://github.com/garmin/LIDARLite_v3_Arduino_Library) on GitHub
    * [Pulsed Light software library](https://github.com/PulsedLight3D) on GitHub

## Connection options

All versions of the LIDAR-Lite unit support both I2C (a two-wire bus interface) and PWM output.

### I2C

The I2C interface is very flexible and easy to use, but has been flakey when connected to the FRC roboRIO. I suspect the root cause is having pull-up resistors on the clock and data lines of _both_ the LIDAR-Lite and the roboRIO, but I haven't fully diagnosed it myself. [Chief Delphi](https://www.chiefdelphi.com/) has many discussions and potential work arounds. In the end, none of them worked out for us and we switched to the PWM connection option.

### PWM Output

Using an alternate set of pins, the LIDAR-Lite can also produce a pulse-width modulated (PWM) output signal. This just means it raises the logic level of a digital output pin for a time that's in proportion to the measured distance. Specifically, the output goes high for 10 microseconds per centimeter of measured distance. This is easy to wire to the roboRIO, requiring only a single resistor and one DIO port in the minimal configuration.

Reading the duration of the PWM output is actually very simple on the roboRIO because its FPGA implements the necessary level detectors and timers. Many people on Chief Delphi have suggested converting the signal to analog, but I strongly disagree with that approach since it loses accuracy.

## Minimal roboRIO Interface

Here's the simplest way to connect the LIDAR-Lite to the roboRIO using PWM mode. It uses just one DIO port.

    roboRIO
    DIO Port                           LIDAR-Lite
    --------                           --------
    Power  __________________________  +5V Power

    Signal __________________________  Mode Select
                   | 1KOhm
                   |_/\/\/\_
    Ground _________________|________  Ground

I'm not including pin numbers for the LIDAR-Lite cable, only pin names, because it varies between version 1, 2, and 3 of the product.

## Source Code

Here's a simple Java class to represent the LIDAR-Lite unit, consisting only of the constructor where the port number is specified and a "get()" method to obtain a reading. This isn't final-quality code in terms of error handling and calibration constants, but should get you started.

    package org.usfirst.frc.team3504.robot;

    import edu.wpi.first.wpilibj.Counter;
    import edu.wpi.first.wpilibj.DigitalSource;

    public class LidarLitePWM {
	/*
	 * Adjust the Calibration Offset to compensate for differences in each unit.
	 * We've found this is a reasonably constant value for readings in the 25 cm to 600 cm range.
	 * You can also use the offset to zero out the distance between the sensor and edge of the robot.
	 */
	private static final int CALIBRATION_OFFSET = -18;

	private Counter counter;
	private int printedWarningCount = 5;
	
	/**
	 * Create an object for a LIDAR-Lite attached to some digital input on the roboRIO
	 * 
	 * @param source The DigitalInput or DigitalSource where the LIDAR-Lite is attached (ex: new DigitalInput(9))
	 */
	public LidarLitePWM (DigitalSource source) {
		counter = new Counter(source);
	    counter.setMaxPeriod(1.0);
	    // Configure for measuring rising to falling pulses
	    counter.setSemiPeriodMode(true);
	    counter.reset();
	}

	/**
	 * Take a measurement and return the distance in cm
	 * 
	 * @return Distance in cm
	 */
	public double getDistance() {
		double cm;
		/* If we haven't seen the first rising to falling pulse, then we have no measurement.
		 * This happens when there is no LIDAR-Lite plugged in, btw.
		 */
		if (counter.get() < 1) {
			if (printedWarningCount-- > 0) {
				System.out.println("LidarLitePWM: waiting for distance measurement");
			}
			return 0;
		}
		/* getPeriod returns time in seconds. The hardware resolution is microseconds.
		 * The LIDAR-Lite unit sends a high signal for 10 microseconds per cm of distance.
		 */
		cm = (counter.getPeriod() * 1000000.0 / 10.0) + CALIBRATION_OFFSET;
		return cm;
	}
    }
