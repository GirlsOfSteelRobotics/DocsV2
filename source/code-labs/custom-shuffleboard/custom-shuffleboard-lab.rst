.. _custom-shuffleboard-lab:

Custom Shuffleboard Widget Lab
==============================
We frequently use Shuffleboard to debug and verify our code, especially in the simulation environment. The text boxes
and boolean indicators can help us out a lot, but sometimes it is better to have a more complicated, customized view to
look at.

Shuffleboard gives us the ability to create our own visualizations, written in Java, which communicate over the same
Network Tables interface used for the rest of shuffleboard. This allows us to do things like plot our robot position on
the field, or even make a simplistic view of our robot, where we can watch our mechanisms move just like they do in real
life.

This provides us with multiple benefits. When we are developing complicated things like autonomous modes in the simulation
environment, it is way easier to look at a "video" of the robot move its elevator up to the correct height, then eject balls
than it is to look at a text box with the elevator height rising, and then a light very quickly turn from red to green when
the solenoid switches state. It can also help debug wiring problems the first time we bring the software up. For example,
"When I hit X the intake motors are supposed to move, and when I hit Y the shooter is supposed to start, but it is backwards.
The Widget says it is doing the correct thing, it must be wired incorrectly"

We will be making a custom widget for the robot we created in the previous code lab. Like that codelab, lots of stubs
have been created which we will need to fill in.

Introduction into JavaFX
------------------------
JavaFX is a framework built on top of java, used to create graphics. You can make very complicated GUI (graphical
user interfaces) complete with buttons, text fields, dropdown boxes, custom graphics, etc. For our shuffleboard purposes
however, we will stick to drawing shapes, typically just :code:`Rectangle` and :code:`Circle` objects for simplicity. When the state of the robot changes, we can change the colors of these shapes, move them around in the image, change
their sizes, and even rotate them around an angle.

JavaFX uses a concept called mvc_ (Model - View - Controller), where the View (what shapes we have) are
defined in their own file (typically a :code:`*.fxml` file in the resources package), the Model (the data structure definitions)
in a separate java file (typically called :code:`*Data.java`), and the Controller (typically in a :code:`*Controller.java`), which takes
in changes in the model and updates the view accordingly.

We have written a script that automatically generates the model from a config file, since shuffleboard requires a LOT
of boiler plate code, and we will just need to focus on what shapes we need to draw (view) and how draw them to reflect
the state of the robot (controller).

Making a View
-------------
The project already contains our view (:code:`src/resources/shuffleboard_codelab.sd_widgets.ss.codelab_super_structure.fxml`),
but we need to add our shapes to it. We can do it by adding all the rectangles and circles we need inside of the group tags, like so

.. code-block:: xml

    <Group fx:id="m_group">
        <children>
            <Circle fx:id="m_spinningWheel" fill="plum"/>
            <Rectangle fx:id="m_punch" fill="peachPuff"/>
        </children>
    </Group>

Note the :code:`fx:id="...."`. The ID is used to auto-magically let the controller and the view talk to each other. So
the id should be whatever you want the member variable name to be. Remember, our coding standard says member variables
should start with :code:`m_`

You also can play around with the :code:`fill`. JavaFX provides a LOT of predefined colors, you can use intellisense
to see what options are available. It usually helps debugging the layout if you make each item a different color, so you
know which thing you are moving around. Unfortunately, we will usually override these colors with more standard values
when we actually connect it to the model.

For this lab, we will want:
- :code:`Rectangle` for the chassis base
- :code:`Rectangle` for the elevator
- A small :code:`Rectangle` for each of the two limit switches for the elevator
- :code:`Rectangle` for the punch solenoid
- :code:`Circle` for the spinning wheel


Setting up the Controller
-------------------------
The first step in writing the controller is defining all the member variables for the shapes you defined in FXML, like
so


.. code-block:: java

    @FXML
    public Circle m_spinningWheel;

    @FXML
    public Rectangle m_punch;

Note the special :code:`@FXML` annotation; this is what allows the black magic connection of the things in your FXML file
and your java file. You have to be careful and make sure your names match the ids in the file, there will be no compiler
errors if you make a mistake, but you will likely get a :code:`NullPointerException` when you try to run it, which can be
confusing and hard to track down.

Next, you will want to move the shapes around and make them the appropriate size. There is a special :code:`initialize`
function, which auto-magically gets called after the FXML file has been loaded, so you should do your work in there.

For the most part, we will only need to set four attributes for :code:`Rectangle`, the X and Y (top left right corner),
and its height and width. For a :code:`Circle`, we need to set its Center X and Y location, and its Radius. For simplicity,
we can use inches for these values, and scale the shapes to make it fit inside of our window (the scaling code is provided)

Note: Computer graphics typically put the origin point at the top left corner of your screen. Moving down the screen
increases your Y value, moving to the right increases your X value. For example, if you had a screen that was 200x100
pixels wide, the coordinate system would look like below.

.. code-block::

   (0,0)                                     (200, 0)
   -------------------------------------------
   |                                         |
   |                                         |
   |                                         |
   |                 (100, 50)               |
   |                     X                   |
   |                                         |
   |                                         |
   |                                         |
   |                                         |
   -------------------------------------------
   (0, 100)                                   (200, 100)


This, combined with the fact that we specify the top left coordinates for rectangles can make it tricky to think about
how to draw an elevator taller, as you will need to change both the height and the Y location


.. code-block:: java

        m_spinningWheel.setRadius(2);
        m_spinningWheel.setCenterX(10);
        m_spinningWheel.setCenterY(20);

        m_punch.setWidth(5);
        m_punch.setHeight(7);
        m_punch.setX(30);
        m_punch.setY(25);

You should now try to place and size all of the objects until it looks like the sample image (except you can substitute
the colors for whatever your heart desires)


Making the Widget Dynamic
-------------------------
Now that we have all of our components in the correct place, we can start making them move to reflect the state of the
robot.

You will notice that there are some stubs that get called when the robot state changes, like :code:`updatePunch`.
We can ask the function argument **IS* the **PUNCH EXTENDED**\ ? If it is, we can change the punches rectangle, so it
looks like it is bigger to reflect the extension.

Similarly, we can change the color of these shapes when they are moving. For example, in the :code:`updateSpinningWheel`
function, we can **GET** the **MOTOR SPEED**, and use the following utility to color it based on how fast it is going.

.. code-block:: java

   m_spinningWheel.setFill(Utils.getMotorColor(spinningWheelData.getMotorSpeed()));

We want the widget to do the following things:
- Change the color of the elevator box based on the motor speed
- Change the size of the elevator rectangle based on the height
- Make the limit switches green if they are at their limit, or black if they are not
- Make the punch bigger if it is extended, or put it back to its default height if it is retracted
- Change the color of the spinning wheel based on the motor speed

.. _mvc: https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller