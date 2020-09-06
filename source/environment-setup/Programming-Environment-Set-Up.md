# FRC Programming Environment

Joseph Jackson

The official FIRST documentation for installing the FRC programming tools is a bit convoluted and time-consuming. I've created a streamlined process for Girls of Steel by:
  * downloading and unpacking all the files ahead of time,
  * focusing only on Java and the third-party libraries we rely on,
  * writing scripts to automate many of the steps on the Mac, and
  * adding steps for using GitHub to share code.

***

1. Obtain a USB drive with all the installers
    * See Joe or the Programming Lead to borrow one. The contents of the drive are documented on the [[Software installers kept on USB Drive]] page.

1. Follow the steps specific to your platform of choice
    1. macOS (the red pill)
        1. Open the `Mac` folder on the USB drive
            * There's no need to make a copy of the folder unless others are waiting for a flash drive
        1. Drag `Visual Studio Code` to your Applications folder
            * As a shortcut, you can drop it on the `Applications alias` found in the Mac folder
            * If prompted, choose to `Replace` any older version
        1. Drag `GRIP` to your Applications folder, same as above
        1. Launch (double-click) the `WPILib-Installer.command` script
            * A Terminal window will open to show the progress as the script runs
            * Confirm there aren't any error messages; If so, show them to Joe
            * Close the Terminal window after you see the `[Process completed]` message
            * If you see an error "cannot be opened because the developer cannot be verified", take these steps:
                * Open `System Preferences` from the Apple menu
                * Go to the `Security & Privacy` preference pane
                * Switch to the `General` tab
                * Click the `Open Anyway` button, if present
                * Back in the error dialog, click `Cancel`
                * Let the installer script finish, but be sure to run it again to complete any canceled steps
        1. Launch (double-click) the `CTRE-Installer.command` script
            * A Terminal window will open to show the progress as the script runs
            * Confirm there aren't any error messages; If so, show them to Joe
            * Close the Terminal window after you see the `[Process completed]` message

    1. Windows (the blue pill)
        1. Open the `Windows` folder on the USB drive
            * There's no need to make a copy of the folder unless others are waiting for a flash drive
        1. Run (double-click) the WPI library installer: `WPILibInstaller_Windows64-*.exe`
            * It is recommended to install for `All Users` when prompted
            * In the main installer window, click on `Select/Download VS Code`, then `Select Existing Download`
            * Select the `OfflineVsCodeFiles-*.zip` from the `Windows` folder of the USB drive
            * Make sure the checkbox for `Visual Studio Code` is checked
            * Click `Execute Install` and wait for the installer to finish
        1. Run the National Instruments FRC Game Tools installer
            * Open the `ni-frc-2020-game` folder found in the `Windows` folder of the USB drive
            * Run `install.exe` to begin the installation
            * Review and accept the license agreements to continue
            * Take all the default installation options by clicking `Next` buttons
            * When prompted to log into your NI User Account, use the `Create account` link, if necessary
            * **NOTE:** When the NI Licensing Wizard prompts you to activate the Vision Development Module, use the `X` button in the upper right to cancel out of the window
            * Reboot when the installer offers the `Reboot Now` option
        1. Run the FRC Radio Configuration installer: `FRC_Radio_Configuration-*.exe` 
            * Accept any default installation options
            * You will be prompted to install a network packet utility as subtask of this installer
            * Be sure to choose `I want to reboot later`, as instructed by the installer
        1. Run the Git installer: `Git-*.exe`
            * There are many, many pages of options presented to customize your Git environment
            * Just accept all defaults and click through the pages to complete the installation
        1. Run the GRIP installer: `GRIP-*.exe` 
            * Accept any default installation options
            * Note that the GRIP tool is automatically launched after installation
            * Quit out of GRIP before continuing
        1. Run the CTRE Phoenix installer: `CTRE Phoenix Framework *.exe` 
            * De-select the Hero C# and LabView options, leaving the others enabled
            * This provides support for the Talon SRX motor controllers
        1. Run the Spark Max installer: `spark-max-client-setup-*.exe` 
            * Includes a utility and libraries for the Spark Max motor controller
        1. (Optional) Install the Limelight firmware flashing tools
            * You can skip this for personal laptops (unless you work with the Limelight often and want it)
            * All team laptops should have this installed
            * Run the USB Flash Driver installer (it's for the embedded Raspberry Pi): `rpiboot_setup.exe`
            * Copy the Balena Etcher imaging tool to the laptop: `balenaEtcher-Portable-*.exe`
              * You can leave it in your Downloads folder or create a new folder such as: `C:\Program Files\Limelight Image Tool\`
              * Right-click on *your copy of* the balenaEtcher* file and create a shortcut to it on your Desktop
            * Copy the latest image file to the same location on your laptop as the imaging tool: `LL_*_RELEASE.zip`
1. [Mac & Windows] Essential VS Code settings
    1. Start `Visual Studio Code` if it's not already running
        * On the Mac, it is in Applications
        * On Windows, you **must** use the desktop icon `FRC VS Code 2020` to start the correct version of VS Code
    1. Bring up the Command Palette by pressing:
        * On the Mac, Command-Shift-P
        * On Windows, Control-Shift-P
    1. In the palette, enter the command `WPILib: Set VS Code Java to FRC Home` and press return or select from the list of matching commands

1. First-time import (clone) of projects from GitHub
    1. Run through this process once per season to establish access to this year's GitHub code repository
        * If you've done this during the preseason, there's no need to do it again
    1. Start `Visual Studio Code` if it's not already running
        * On the Mac, it is in Applications
        * On Windows, you **must** use the desktop icon `FRC VS Code 2019` to start the correct version of VS Code
    1. Bring up the Command Palette by pressing:
        * On the Mac, Command-Shift-P
        * On Windows, Control-Shift-P
    1. In the palette, enter the command `Git: Clone` and press return or select from the list of matching commands
    1. Enter in the URL for this year's GitHub repository:
        * `https://github.com/GirlsOfSteelRobotics/2020GirlsofSteel.git`
        * (Adjust the URL above for the current season, if necessary)
    1. Select a location to create a new folder for the repository
        * Your `Documents` folder is a reasonable place to put it
    1. If prompted to open the repository, cancel out with the `X` button
        * **Only open individual VS Code projects** found inside the repo instead of opening the entire repo!
    1. To open a project, use the `Open Folder` option from the VS Code Welcome page or from the `File` menu

1. Create a personal GitHub account
    * <http://github.com/>
    * If you already have one for school, there's no need to create another
    * Ask Joe or the Programming Lead to invite your GitHub account to the Contributors team for Girls of Steel
    * See your e-mail to accept the invitation
    * When finished, you can push changes to GitHub to share them with the rest of the team
    * VS Code will prompt you for your GitHub username and password the first time you push code

1. Installing Third-Party Libraries
    When using third-party hardware on the robot, the project needs to have the associated libraries installed. <Fill in more details here.>