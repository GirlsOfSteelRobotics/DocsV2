.. _gitflow-lab:

Gitflow Lab
===========

This lab will walk you through our workflow, from creating branches through merging a pull request.

Here is the project we will be using https://github.com/GirlsOfSteelRobotics/Git-Workflow-Codelab

Part One
________

1. Checkout a new branch
------------------------
IMPORTANT: For this code lab, you will base your branch off of something other than master. This is to force a merge conflict later in the lab. 99.9% of the time you are developing real code from the robot, you would start off of master


Base the branch off of origin/codelab_start, and name it something like <your name>_codelab_part1

2. Create a new Subsystem
-------------------------
Create it with the name "<your name>Codelab<year>Part1", ex. :code:`PJCodelab2020Part1`

3. Put a print line in the constructor
--------------------------------------
Something along the line of :code:`System.out.println("<name> says hello world in <year> part 1");`

4. Create your command
----------------------
In :code:`RobotContainer`, declare your subsystem.

5. Run SnobotSim
----------------
Run it from the run configurations area, and make sure your string gets printed out

6. Commit, Push, Create PR
--------------------------
You will notice that the you cannot merge your branch, because there is a conflict

7. Fix conflict, re-push
------------------------
After the push, add PJ or Joe as a reviewer, and ping them in Slack to review and approve the PR

Part Two
________
Part two is meant to make sure you run the cleanup process correctly, and can create your next feature branch.

Re-run steps 1-6, but replace any references to "part1" with "part2"
