.. _style-guide:


Style Guide
===========

Our styleguide is enforced by a Checkstyle_ configuration file. This ensures that we all follow the same conventions
for variable names, class names, spacing, brace location, and many other things.

While the styleguide enforces many more rules than outlined below, these are some of the major ones to be aware of.

Class Names
-----------
Class names should be UpperCamelCase, i.e. :code:`ShooterSubsystem`

Function Names
--------------
Function names should be lowerCamelCase, i.e. :code:`void setSpeedAndSteer(double speed, double steer)`

Variable Names
--------------

- Member variables (variables that belong to a class) should start with the `m_` prefix, then be lowerCamelCase after that, i.e. :code:`TalonSRX m_leftDriveMotor`
- Local variables and function arguments should be lowerCamelCase
- Constants should be be :code:`static final`, and be named UPPER_SNAKE_CASE, i.e. :code:`public static final double DEFAULT_SHOOTER_RPM = 5000;`

Braces and Spacing
------------------

- All keywords (i.e :code:`if, switch, for`, etc) and math operands (:code:`+, -, <`, etc) should be seperated by a space

    .. code-block:: java

        if (x > 5) // Good

        if(x>5) // Bad!

- Indentation should be done with 4 spaces. Never 2, never with Tabs

- We use `K&R Braces`_, meaning that all opening braces start on the same line.

    .. code-block:: java
    
        String myGoodFunction(int x) {
            if (x > 5) {
                return "Greater Than 5"
            }
            else if (x == 5) {
                return "Equals 5";
            }
            else {
                return "Less than 5";
            }
        }

        String badFunctionButPJLikesItMore(int x)
        {
            if (x > 5) 
            {
                return "Greater Than 5"
            }
            else if (x == 5) 
            {
                return "Equals 5";
            }
            else 
            {
                return "Less than 5";
            }
        }


.. _Checkstyle: https://checkstyle.sourceforge.io/
.. _K&R Braces: https://en.wikipedia.org/wiki/Indentation_style#K&R_style