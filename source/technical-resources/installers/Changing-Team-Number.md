## Changing the Team Number

We use alternate team numbers during summer camps and off-season events, so occasionally have a need to change them. The steps are:

1. Update the FRC plug-in in your IDE:
   a. In Eclipse, from the Window or Eclipse menu, choose Preferences. The team number is under "WPILib Preferences".
   b. In VSCode, use Control-P (Command-P on the Mac) (maybe with Shift?), then enter the command "WPILib: Set Team Number"
   c. In IntelliJ, the New Project wizard for FRC prompts for a team number and stores it with the project. 
      To update, manually edit the wpilib_preferences.json found in the ".wpilib" folder at the top of the project.
3. In the SmartDashboard, the team number is in the Preferences.
4. Use the roboRIO Imaging Tool to update the team number. Formatting is not necessary as long as the image is up to date.
5. Use the FRC Radio Configuration Utility to change the team number. Loading firmware is not necessary.
