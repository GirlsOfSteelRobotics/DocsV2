.. _monorepo-intro:

Monorepo intro
==============

We now use a monorepo, aka we keep all of our code in a single repository instead of creating a new 
one every year. This allows us to easily reference our old code, and quickly up-convert our previous
years robots to the latest version of WPILib without having to copy robot projects between repositories.

However, that does make some of the "one time" tasks like creating a project a little bit more involved, 
and we are forced to make sure every project compiles with the latest versions of the external libraries.


.. toctree::
   :maxdepth: -1
   :caption: stuff:

   create-new-robot-project
   create-new-shuffleboard-plugin
