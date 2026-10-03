.. _gitflow-lab:

Gitflow Lab
===========

This lab will walk you through our workflow, from creating branches through merging a pull request.

Part One
________

1. Checkout a new branch
------------------------
In the terminal, run something like :code:`git checkout -b <your_name>_codelab_part1`

Note:
:code:`git checkout ...` checks out a branch, and adding :code:`-b <branch_name>` makes a new branch first, then checks it out



2. Create a new Subsystem
-------------------------
Create it with the name "<your name>Codelab<year>Part1", ex. :code:`PJCodelab2020Part1`

3. Put a print line in the constructor
--------------------------------------
Something along the line of :code:`System.out.println("<name> says hello world in <year> part 1");`

4. Create your command
----------------------
In :code:`RobotContainer`, declare your subsystem.

5. Run the simulator
--------------------
Run it from the run configurations area, and make sure your string gets printed out

6. Commit, Push, Create PR
--------------------------
You will notice that the you cannot merge your branch, because there is a conflict

7. Fix conflict, re-push
------------------------
After the push, add PJ or Joe as a reviewer, and ping them in Slack to review and approve the PR
