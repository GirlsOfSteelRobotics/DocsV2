.. _general-guidelines:

General Guidelines
==================

- In general, there should be a 1:1 match between mechanical subsystems and our software subsystems.
- Base commands should be broken down into extremely simple tasks. More complicated commands can be strung together
  with :code:`ParallelCommandGroup` or :code:`SequentialCommandGroup`, which should be represented in their own class
- Fully fledged autonomous modes can be represented as a command group, but should live in a different package than the rest of the commands
- Constants that need to be tuned such as PIDF values should use our `Properties` library. This allows us to quickly tune the value on 
  Shuffleboard, and easily lock them once we are satisfied with the values. This dramatically reduces debugging time, since you don't have to
  rebuild and redeploy the code during tuning.
- There should be no "Magic Numbers" in the code. All motor/sensor ID's should be defined in `Constants.java`, and all other constants should be defined inside the class that is using them
- Always test your code with the simulator or the real robot before you declare your task "done" and ready for review.
- ALWAYS commit your code before you leave a meeting.