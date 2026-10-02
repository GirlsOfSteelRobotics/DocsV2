.. _simulator-lab:

Simulator Lab
=============

This lab will get you acquainted with using the simulator, and executing some basic robot controls.
Many parts of the robot code have been "stubbed out", meaning that much of the infrastructure work
you would normally have to write already exists, so that you can just focus on the fun part of making
the robot do things.

All of the places that need to be updated are marked with :code:`// TODO implement`, which can be seen in the TODO tab of
Intellij. This lab will walk through the easiest to most complex tasks in order, but feel free to
jump around if you are comfortable.

This project also contains unit tests. Unit tests run the robot code in a simulated world and check that
certain expectations are met, for example "when I tell the elevator to go up, its height increases".
In order to "pass" this codelab, all of the unit tests should pass.

Getting Started
---------------
The codelab lives in our main robot code repository, https://github.com/GirlsOfSteelRobotics/GirlsOfSteelFRC, in the
:code:`codelabs/basic_simulator` folder. All of the code is in the :code:`com.gos.codelabs.basic_simulator` package,
and the tests are in :code:`src/test/java`.

Before you start, create a branch to do your work on, like you did in the :ref:`gitflow-lab`, for example
:code:`git checkout -b <your_name>_simulator_codelab`

There are two run configurations (in the "Codelabs" folder of the run configurations dropdown) that you will use:

- **Codelabs - Basic Simulator - Run Tests** runs all of the unit tests
- **Codelabs - Basic Simulator - Simulate** starts the simulator, so you can drive the robot around

You can also run a single test class or a single test by clicking the green arrow next to it in the editor.
This is much faster than running everything, and is the best way to check your work after each step.

A couple of things to know about the code:

- Distances are always in **meters**, angles are in **degrees**, and wheel speeds are in **RPM** (rotations per minute).
  WPILib has a helper class, :code:`Units`, which can convert between units, for example :code:`Units.inchesToMeters(10)`
- Our :ref:`direction-conventions` apply here: driving forwards, moving the elevator up, and extending the punch are all
  "positive", and angles get bigger as the robot turns counter-clockwise (to the left)

Robot Overview
--------------
We will be implementing code for a very simple robot.

The robot has the following mechanisms (think: Subsystems):

- A Chassis, with 4 motor controllers (2 each side), an encoder for each side, and a gyroscope
- An Elevator, with one motor controller, an encoder, and limit switches on the top and bottom
- A Punch, which uses a single action solenoid
- A Shooter, which is a spinning wheel with one motor controller and an encoder

Our goal is to give the robot these abilities (think: Commands):

- Drive the chassis with the driver's Xbox controller, using Halo drive (aka split arcade, aka left thumb throttle, right thumb rotation)
- Drive the elevator with a joystick on the operator's Xbox controller
- Have buttons which move the elevator to 3 preset heights
- Have a button to move the punch piston
- Have a button to spin the shooter wheel at a preset RPM
- Autonomously drive the robot straight for N seconds
- Autonomously drive the robot straight for N meters
- Autonomously turn the robot to N degrees
- An autonomous mode that combines these together

Implement the Punch Subsystem
-----------------------------
The punch subsystem uses a pneumatic solenoid to control our piston.
Pneumatics can only have two states, extended vs retracted (in vs out). Because of this,
we interact with them using the :code:`boolean` values :code:`true` or :code:`false`.

If we look at our :code:`PunchSubsystem` class, we can see the member variable :code:`m_punchSolenoid`,
and the functions :code:`extend` and :code:`retract`. Inside of these functions, we will want to
**SET** the solenoid to a certain state. Our coding standard states that the extended state should be :code:`true`,
and the retracted state should be :code:`false`. We should also fill out the :code:`isExtended` function, by asking for
and **GET**\ ing the current state.

Once that is complete, you can try running the :code:`PunchSubsystemTest`, and see if you have
implemented things correctly. Once the tests pass, make a commit.

Implement the Move Punch Command
--------------------------------
Now we can make our first command. The :code:`MovePunchCommand` is told whether it should extend or retract the punch
when it is created, and saves that in :code:`m_extendPunch`. When the command **EXECUTE**\ s, it should either call
:code:`extend` or :code:`retract` on the punch subsystem, depending on that value.

Once that is complete, you can run the :code:`MovePunchCommandTest`. Once the tests pass, make a commit.

Implement the Elevator Manual Controls
--------------------------------------
The elevator subsystem uses a motor controller to make the physical motor spin. Motors can go multiple
different speeds, and the library provides a nice API where we can tell the motor to go full speed forwards
by **SET**\ ing it to 1.0, and making it go full speed backwards by **SET**\ ing it to -1.0. We can
also do any speed in between, like going half speed forwards by **SET**\ ing it to 0.5, or making it go
backwards at 10% speed by **SET**\ ing it to -0.1. For the elevator, "forwards" means up.

For manual control, we have a function called :code:`setSpeed`, and we will give that value
directly to the motor controller, :code:`m_liftMotor`. The :code:`stop` function should **SET** the motor to 0.

The elevator also has an encoder, :code:`m_liftEncoder`, and we can ask it for its current position. We should fill out
the :code:`getHeight` function, and return whatever the value is when we **GET** the **POSITION**. The encoder has
already been set up so that the position is the height of the elevator in meters.

Finally, fill out :code:`isAtLowerLimit` and :code:`isAtUpperLimit` by **GET**\ ing the value of the limit switches.
They return :code:`true` when the switch is pressed.

Once that is complete, you can try running the :code:`ElevatorSubsystemTest.testManuallyMoveUp` and
:code:`ElevatorSubsystemTest.testManuallyMoveDown` tests. Once the tests pass, make a commit. Don't worry about
:code:`testGoToPosition` yet, we will do that later.

Implement the Chassis Manual Controls
-------------------------------------
Similar to the elevator, the chassis has a motor controller and an encoder for each side. However, the FIRST library
provides a nice helper class, :code:`DifferentialDrive` which does some fancy math to help us implement our
arcade drive functionality, so we won't be **SET**\ ing the motor controllers directly, but rather talking to that class's
:code:`arcadeDrive` function, using the :code:`m_differentialDrive` member variable. This function takes in a speed
(aka throttle, aka how fast we want to drive straight), and a rotation value (how much we want to curve/spin).
Like the rest of WPILib, a positive rotation turns the robot counter-clockwise (to the left).

For our :code:`arcadeDrive` function, we can pass the :code:`speed` and :code:`steer` arguments straight through to
:code:`m_differentialDrive`. For :code:`stop`, :code:`DifferentialDrive` has a :code:`stopMotor` function.

We will also want to fill out the :code:`getLeftDistance` and :code:`getRightDistance` functions, like we did with the elevator.
You will need to do a little bit of simple math to fill out the :code:`getAverageDistance` function, but it should incorporate
the distance of both the left and right sides.

Lastly, we need to know which way the robot is facing for :code:`getHeading`. The gyroscope, :code:`m_gyro`, can give us
this. Be careful, because the gyro's :code:`getAngle` function counts **clockwise** as positive, which is the opposite of
everything else. Instead, you can use :code:`m_gyro.getRotation2d().getDegrees()`, which counts counter-clockwise as positive
like we want.

Once that is complete, you can try running the :code:`ChassisSubsystemTest`. Once the tests pass, make a commit.

Implementing Joystick Interactions
----------------------------------
Up to this point, we added the ability for the chassis and elevator to move with manual input, and now
we want to ask our Xbox controllers for what that manual input should be. For example, the more we press the
driving joystick forward, the faster we want to make the robot drive. Luckily, the joysticks are also normalized
to a [-1.0, 1.0] range, so the mapping is easy.

To do this, we go into the :code:`DriveChassisWithJoystickCommand` and :code:`ElevatorWithJoystickCommand` commands. Each one
is given the :code:`CommandXboxController` it should listen to. When they **EXECUTE**:

- The chassis should call :code:`arcadeDrive`. The speed should come from the **LEFT** stick's **Y** axis (:code:`getLeftY`),
  and the steering should come from the **RIGHT** stick's **X** axis (:code:`getRightX`).
- The elevator should call :code:`setSpeed`, with the value from the **RIGHT** stick's **Y** axis (:code:`getRightY`).

**IMPORTANT NOTE** The joysticks don't use the same directions as the robot, so you will need to negate (put a minus sign
in front of) all of these values:

- For historical reasons, due to how people like to fly airplanes, pushing forwards on the Y axis gives a negative value,
  and pulling back gives a positive value. We want pushing forwards to drive forwards, and move the elevator up.
- Pushing right on the X axis gives a positive value, but we want pushing right to turn the robot to the right, which is
  clockwise, which is negative.

Now, our commands need to run whenever nothing else is using the subsystem. We do that by making them the subsystem's
"default command". In :code:`RobotContainer`, inside of :code:`configureButtonBindings`, call :code:`setDefaultCommand` on the
chassis and elevator subsystems, creating the commands with the driver joystick (:code:`m_driverJoystick`) and the operator
joystick (:code:`m_operatorJoystick`). For example:

.. code-block:: java

   m_chassisSubsystem.setDefaultCommand(new DriveChassisWithJoystickCommand(m_chassisSubsystem, m_driverJoystick));

Once those are hooked up, you can run the :code:`DriveChassisWithJoystickCommandTest` and :code:`ElevatorWithJoystickCommandTest` tests.
Once those pass, make a commit.

Implement Driving with Timers
-----------------------------
Most of the time in autonomous, we want finer grained controls than just "drive forwards for a couple seconds and hope
we don't hit a wall", but it is always a good command to have in our back pocket in case all of our sensors break.

To do this, the :code:`AutoDriveStraightTimedCommand` will drive straight with some speed (either forwards or backwards), for an
amount of time. WPILib provides a :code:`Timer`, :code:`m_timer`, that we can **RESTART** when our command **INITIALIZE**\ s.
Each time our command **EXECUTE**\ s we can call :code:`arcadeDrive` with our speed argument, and no steering.
Our command **IS FINISHED** when the timer **HAS ELAPSED** our time argument.

Note, it is important that when we **END** our command, we stop driving, otherwise the chassis
will keep on trucking after our timer has expired.

Once those functions have been filled out, you can run the :code:`AutoDriveStraightTimedCommandTest`. Once the
tests pass, make a commit.

Implement Driving a Distance
----------------------------
More often, we will want to tell the robot to drive some distance in autonomous mode.

To **EXECUTE** the :code:`AutoDriveStraightDistanceCommand`, we will want to figure out how far away we are from our goal
(the **ERROR**, which is the goal distance minus our current **AVERAGE DISTANCE**), and save it in :code:`m_error`.
If the error is positive, we need to drive forwards (positive throttle), and if it is negative, we need to drive backwards
(negative throttle). A speed around 0.5 works well. We can say we are **FINISHED** when we are close enough to our goal distance.
Like the previous command, it is important that when we **END** our command, we stop driving.

**IMPORTANT NOTE** We will never hit our goal right on the nose. Doing a :code:`current == goal` check will (pretty much)
always fail, because if we are even off by the width of a single atom, we aren't there. Usually it is good
to set up some kind of **ALLOWABLE ERROR**, and check if we are inside of that window. The command already has an
:code:`ALLOWABLE_ERROR` constant, and we want to make sure we are within :code:`-ALLOWABLE_ERROR < error < ALLOWABLE_ERROR`.
Using something like :code:`Math.abs` makes that check pretty easy.

Once that is complete, you can run the :code:`AutoDriveStraightDistanceCommandTest` tests. Once those pass,
make a commit.

Implement Moving the Elevator to a Height
-----------------------------------------
Much like "drive straight a distance", we want our elevator to move up or down until we have
reached our goal height. Since we will use this in a couple of places, we put the logic in the :code:`ElevatorSubsystem`'s
:code:`goToPosition` function. It gets called every loop, and should:

- Figure out the error between the goal height and the current height
- If we are within :code:`ALLOWABLE_POSITION_ERROR` of the goal, stop the motor and return :code:`true`
- Otherwise, **SET** the **SPEED** to move up or down towards the goal (around 0.5 works well), and return :code:`false`

You can now run the :code:`ElevatorSubsystemTest.testGoToPosition` test.

Then, in the :code:`ElevatorToPositionCommand`, we can **EXECUTE** by calling :code:`goToPosition` and saving whether we
got there in :code:`m_finished`. When we **END** the command, we should stop the elevator.

This command has one extra feature, the :code:`m_holdAtPosition` variable. Elevators fall back down because of gravity
when the motor stops, so sometimes we want the command to keep holding the elevator at the goal height until someone
interrupts it. So, the command **IS FINISHED** when it has gotten to the goal (:code:`m_finished`) **AND** it is not supposed to hold
(:code:`!m_holdAtPosition`).

Once that is complete, you can run the :code:`ElevatorToPositionCommandTest`. Once those pass, make a commit.

Implement Turning to an Angle
-----------------------------
The :code:`TurnToAngleCommand` spins the robot in place until it faces a goal angle. This works almost exactly like driving
a distance, except we use the **HEADING** instead of the **AVERAGE DISTANCE**, and we **STEER** instead of driving forwards.
Remember that a positive error means we need to turn counter-clockwise, which is a positive steer.

You will probably notice that the robot coasts a little bit past the goal after the command stops the motors, because it
is still moving when it gets there. Turning slowly (a steer of about 0.3) makes this smaller. Real robots do the same thing,
and making them stop quickly and accurately is a big part of what PID control (a future lab!) is for.

Once that is complete, you can run the :code:`TurnToAngleCommandTest`. Once those pass, make a commit.

Implement the Shooter
---------------------
The shooter is a spinning wheel, and it works a lot like the elevator. In :code:`ShooterSubsystem`:

- :code:`setSpeed` and :code:`stop` **SET** the motor speed, just like the elevator
- :code:`getRpm` **GET**\ s the **VELOCITY** from the encoder, which has been set up to be in RPM
- :code:`isAtRpm` returns :code:`true` if we are within :code:`ALLOWABLE_RPM_ERROR` of the goal RPM

The fun part is :code:`spinAtRpm`, which gets called every loop to get the wheel up to the goal RPM. Unlike the elevator,
a spinning wheel doesn't need to be stopped exactly at a position, and it slows down on its own, so we can use
a very simple strategy called "bang-bang" control: if the wheel is going slower than the goal, run the motor at full speed (1.0),
and if it is going faster than the goal, turn the motor off (0).

Once that is complete, you can run the :code:`ShooterSubsystemTest`.

Then in the :code:`ShooterRpmCommand`, **EXECUTE** should call :code:`spinAtRpm` with the command's RPM, and **END** should stop
the shooter. This command never finishes on its own, it keeps the wheel spinning until it gets interrupted.

Once that is complete, you can run the :code:`ShooterRpmCommandTest`. Once those pass, make a commit.

Wire Up Commands to Buttons
---------------------------
Now that we have all of our commands implemented and tested, we can hook them up to buttons
on the operator's Xbox controller.

This takes place in :code:`configureButtonBindings` in :code:`RobotContainer`. Our joysticks are :code:`CommandXboxController`\ s,
which have a function for each button, like :code:`a()`, :code:`b()`, or :code:`rightBumper()`. These give us a :code:`Trigger`, which
lets us run commands when a button is pressed, held, or released. For example:

.. code-block:: java

   m_operatorJoystick.a().onTrue(...);

We want the operator joystick to do the following things:

- While B is held, have the elevator go to the :code:`LOW` position, and hold it there
- While Y is held, have the elevator go to the :code:`MID` position, and hold it there
- While X is held, have the elevator go to the :code:`HIGH` position, and hold it there
- When A is pressed, extend the punch. When it is released, retract it
- While the right bumper is held, spin the shooter at :code:`ShooterSubsystem.SHOOTING_RPM`

The elevator positions live in :code:`ElevatorSubsystem.Positions`, and :code:`ElevatorToPositionCommand` has a constructor that
takes a position and whether it should hold, for example :code:`new ElevatorToPositionCommand(m_elevatorSubsystem, ElevatorSubsystem.Positions.LOW, true)`.
When the button is released and the command is interrupted, the joystick default command takes over again.

Once that is complete, you can run the :code:`OITest`. Once those pass, make a commit.

Create an Autonomous Mode
-------------------------
Finally, we can chain our commands together into an autonomous mode. :code:`DriveElevatePunchCommandGroup` is a
:code:`SequentialCommandGroup`, which runs a list of commands one after the other. In its constructor, call :code:`addCommands`
with commands to:

1. Drive forwards 5 feet (remember to convert it to meters!)
2. Move the elevator to the :code:`HIGH` position
3. Extend the punch

Be careful to only use commands that finish on their own, otherwise the group will get stuck waiting forever!

Once that is complete, you can run the :code:`DriveElevatePunchCommandGroupTest`. At this point, every test should pass, so
run all of them with the "Run Tests" configuration to make sure. Make a commit, push your branch, and create a pull request.
