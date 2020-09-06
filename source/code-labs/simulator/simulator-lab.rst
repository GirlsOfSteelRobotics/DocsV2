.. _simulator-lab:

Simulator Lab
=============

This lab will get you acquainted with using the simulator, and executing some basic robot controls.
Many parts of the robot code have been "stubbed out", meaning that much of the infrastructure work
you would normally have to write already exists, so that you can just focus on the fun part of making
the robot do things.

All of the places that need to be updated are marked with TODO's, which can be seen in the TODO tab
Intellij. This lab will walk through the easiest to most complex tasks in order, but feel free to
jump around if you are comfortable.

This project also contains unit tests. Unit tests run automatically when we build, and
test the code and make sure that certain expectations are met. In order to "pass" this codelab,
all of the unit tests should pass

Robot Overview
--------------
We will be implementing code for a very simple robot.

The robot has the following mechanisms (think: Subsystems):
- A Chassis, with 4 speed controllers (2 each side), an encoder for each side, and a gyroscope
- A Elevator, with one speed controller, an encoder, and limit switches on the top and bottom
- A Punch, which uses a single action solenoid
- A Spinning Wheel, with one speed controller and an encoder

Our goal is to give the robot these abilities (think: Commands)
- Drive the chassis with the drivers Xbox controller, using Halo drive (aka split arcade, aka left thumb throttle, right thumb rotation)
- Drive the elevator with a joystick on the operator XBox controller
- Have buttons which allow the elevator to go to 3 preset heights
- Have a button to move the punch piston
- Have a button to spin the wheel at a preset RPM
- Autonomously drive the robot straight for N seconds
- Autonomously drive the robot straight for N feet
- Autonomously turn the robot to N degrees

Implement the Punch Subsystem
-----------------------------
The punch subsystem uses a pneumatic solenoid to control our piston.
Pneumatics can only have two states, extended vs retracted (in vs out). Because of this,
we interact with them using the :code:`boolean` values :code:`true` or :code:`false`

If we look at our :code:`PunchSubsystem` class, we can see the member variable :code:`m_punchSolenoid`,
and the functions :code:`extend` and :code:`retract`. Inside of these functions, we will want to
**SET** the solenoid to to a certain state. Our coding standard states that the extended state should be :code:`true`,
and the retracted state should be :code:`false`. We should also fill out the :code:`isExtended` function, by asking for
and **GET**\ ing the current state

Once that is complete, you can try running the :code:`PunchSubsystemTest`, and see if you have
implemented things correctly. Once the tests pass, make a commit.

Implement the Elevator Manual Controls
--------------------------------------
The elevator subsystem uses a speed controller to make the physical motor spin. Motors can go multiple
different speeds, and the library provides a nice API where we can tell the motors to go full speed backwards
by **SET**\ ing it to 1.0, and making it go full speed backwards by **SET**\ ing it to -1.0. We can
also do any speed in between, like going half speed forwards by **SET**\ ing it to 0.5, or making it go
backwards at 10% speed by **SET**\ ing it to -0.1

For manual control, we make a function, called :code:`setSpeed`, and we will give that value
directly to the speed controller

The elevator also has an encoder, and we can ask it for its current position. We should fill out
the :code:`getHeight` function, and return whatever the value is when we **GET** the **POSITION**

Once that is complete, you can try running the :code:`LiftingSubsystemTest.testManuallyMoveUp` and
:code:`LiftingSubsystemTest.testManuallyMoveDown` tests. Once the tests pass, make a commit.

Implement the Chassis Manual Controls
-------------------------------------
Similar to the elevator, the chassis has a motor controller and an encoder for each side. However, the FIRST library
provides a nice helper class, :code:`DifferentialDrive` which does some fancy math to help us implement our
arcade drive functionality, so we won't be **SET**\ ing the motor controllers directly, but rather talking to that classes
:code:`arcadeDrive` function. This function takes in a speed (aka throttle, aka how fast we want to drive straight), and
a rotation value (how much we want to curve/spin).

We will also want to fill out the :code:`getLeftDistance` and :code:`getRightDistance` functions, like we did with the elevator.
You will need to do a little bit of simple math to fill out the :code:`getAverageDistance` function, but it should incorporate
the distance of both the left and right sides.

Once that is complete, you can try running the :code:`ChassisSubsystemTest`. Once the tests pass, make a commit

Note: You are off the hook for the gyroscope, because the API is too gross to explain for this lab.

Implementing Joystick Interactions
----------------------------------
Up to this point, we added the ability for the chassis and elevator to moe with manual input, and now
we want to ask our xbox controllers for what that manual input should be. For example, the more we press the
driving joystick forward, the faster we want to make the robot driver. Luckily, the joysticks are also normalized
to a [-1.0, 1.0] range, so the mapping is easy.

To do this, we need to implement the functions in :code:`OI`

For :code:`getDriverThrottle` we want to **GET** the **Y** axis for the **LEFT HAND**. For spin,
we want to **GET** the **X** axis on the **RIGHT HAND**. For the elevator, we want to **GET** the
**Y** on the **RIGHT HAND**

**IMPORTANT NOTE** For historical reason, due to how people like to fly airplanes, pushing forwards on the Y axis
will result in a negative value, and pulling back yields a positive value, so you will probably want to negate those values.

Now that we can read the joysticks, we can implement our manual tele-op commands.
To do this, we can go into our :code:`LiftWithJoystick` and :code:`DriveChassisWithJoystick` commands, and call the functions
we implemented in the previous steps.

For the elevator, we want to **SET** the **SPEED**, and give it the values from **GET**\ ing the **ELEVATOR JOYSTICK** from the OI member variable

For the chassis, we want to **SET** the **SPEED** and **STEER**\ ing values, and give it the values from **GET**\ ing the
**DRIVER THROTTLE** and **DRIVER SPIN**.

Once those are hooked up, you can run the :code:`DriveChassisWithJoystickCommandTest` and :code:`LiftWithJoystickCommandTest` tests.
Once those pass, make a commit.

Implement Driving with Timers
-----------------------------
Most of the time in autonomous, we want finer grained controls than just "drive forwards for a couple seconds and hope
we don't hit a wall", but it is always a good command to have in our back pocket in case all of our sensors break.

To do this, we can make a command that will drive straight with some speed (either forwards or backwards), and give it an
amount of time to run. WPILib provides a timer functionality that we can **START** as soon as our command runs, and we
can **GET** how many seconds have elapsed each loop, and say our command **IS FINISHED** after the time is bigger
than our argument. Each time our command **EXECUTE**\ s we can **SET** our **THROTTLE** to whatever our speed argument is.

Note, it is important that when we **END** our command, we stop driving, otherwise the chassis
will keep on trucking after our timer has expired

Once those functions have been filled out, you can run the :code:`AutoDriveStraightTimedCommandTest`. Once the
tests pass, make a commit

Implement Driving a Distance
----------------------------
More often, we will want to tell the robot to drive some number of feet in autonomous mode.

So to **EXECUTE** this command, we will want to check where we currently are (based on our **AVERAGE DISTANCE**, and
figure out if we need to drive forwards (positive throttle) or backwards (negative throttle). We can say we are **FINISHED**
when we are close enough to our goal distance. Like the previous command, it is important that when we **END** our command,
we stop driving.

**IMPORTANT NOTE** We will never hit our goal right on the nose. Doing a :code:`current == goal` check will (pretty much)
always fail, because if we are even off by the width of a single atom, we aren't there. Usually it is good
to set up some kind of **ALLOWABLE ERROR**, and check if we are inside of that window. We want to make sure we are within
[-deadband < error < deadband]. Using something like :code:`Math.abs` makes that check pretty easy.

Once that is complete, you can run the :code:`AutoDriveStraightDistanceTest` tests. Once those pass,
make a commit.

Implement Moving the Elevator to a Height
-----------------------------------------
Much like the "drive straight a distance", we want our elevator to move up or down until we have
reached our goal height. When we **EXECUTE** our command, we will figure out if we need to move up or down,
and we will **SET** the elevator motors **SPEED** accordingly. Also like the chassis, we want to make
sure we stop the motor when we **END** the command.

Once that is complete, you can run the :code:`LiftToPositionCommandTest`. Once those pass, make a commit

Wire Up Commands to Buttons
---------------------------
Now that we have all of our commands implemented and tested, we can hook them up to buttons
on the driver and operators xbox controllers.

This takes place in the :code:`OI` class. We want the operator joystick to do the following things:
- When B is pressed, have the lift go to a position of 0 inches
- When Y is pressed, have the lift go to a position of 40 inches
- When X is pressed, have the lift go to a position of 60 inches
- When A is pressed, extend the punch. When it is released, retract it
- When RB (kBumperLeft) is pressed, make the spinning wheel run.