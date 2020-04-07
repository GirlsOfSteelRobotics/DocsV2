
# Upgrading Firmware and Setting Up Electronics

This is a quick reference guide to upgrading and setting up the FRC electronics. Many of the utilities mentioned are Windows-only.

### RoboRio
1. Upgrade firmware
    1. Connect to the roboRIO with USB
    1. In Internet Explorer (not Firefox, Chrome, or Edge) visit http://172.22.11.2/
    1. Login with "admin" and blank password
    1. Click `Update Firmware` button
    1. Firmware is in "C:\Program Files (x86)\National Instruments\Shared\Firmware\cRIO\76F2\"
1. Install the FRC image
    1. C:\Program Files (x86)\National Instruments\LabVIEW 2016\project\roboRIO Tool\roboRIO_ImagingTool.exe
    1. Assign team number if needed
    1. Check the `Format Target` box if the image listed below it is newer than what's currently installed
1. Add CTRE extensions to the RoboRIO web interface for managing CAN devices
    1. The CAN device tab won't appear in an imaged roboRIO until this step is completed
    1. Connect to the roboRIO with USB
    1. Launch the LifeBoat utility using the taskbar icon or from
    C:\Users\Public\Public Documents\Cross The Road Electronics\LifeBoat\HERO LifeBoat.exe
    1. Switch to the `FRC roboRIO` tab
    1. Press the `Install Phoenix/Web-based Config` button
    1. Quit out of LifeBoat and restart the roboRIO

### PCM, PDP, and CAN Talons

1. Upgrade firmware
    1. Connect to RoboRIO with USB
    1. In Firefox/IE visit http://172.22.11.2/
    1. Login with "admin" and blank password
    1. Select device to be checked or upgraded on left side
    1. If upgrade is needed, firmware files are in
    "C:\Users\Public\Public Documents\FRC"
1. Set CAN ID numbers
    1. Each CAN ID is preset to 0 at the factory
    1. If there are multiple devices of the same type (PCM, PDP, or Talon), each needs a unique ID
    1. Choose one, check the `Light Device LED` box, and press `Save`
    1. Locate the unit with the rapidly flashing light
    1. Enter the appropriate CAN ID (usually based on what's in the code) and press `Save`

### Wi-Fi Radio

After attending a competition, don't forget to reconfigure the radio for home use.

1. Check for Utility updates
    1. Download and run the installer for the latest version of the FRC Radio Configuration Utility from <https://wpilib.screenstepslive.com/s/currentCS/m/getting_started/l/144986-programming-your-radio#download_the_software>

1. Configure an OpenMesh OM5P-AN or OM5P-AC radio for home use
    1. Attach to the radio with an ethernet cable and disable your computer's Wi-Fi
        * If using a USB/Thunderbolt Ethernet adapter, you may need to reboot Windows
    1. Run the Utility located in "C:\Program Files (x86)\FRC Radio Configuration Utility\"
    1. Fill in the team number
    1. Set the appropriate radio type
    1. Leave the mode at "2.4GHz Access Point"
    1. If not already done, update to this year's firmware (otherwise, configuration will fail)
        1. Power off the radio by pulling out the power cord
        1. Press the "Load Firmware" button
        1. When prompted, re-insert the power cord
    1. Fill in a robot name specific to this robot (e.g. PracticeBot)
        1. This avoids having multiple robots with the same Wi-Fi name
        1. The resulting name is composed of the team number and name (e.g. 3504_PracticeBot)
    1. Press the `Configure` button and exit the utility when finished
 
1. Configure a D-Link radio for home use
    1. **NOTE: D-Link radios are no longer supported for competition** as of the 2017 season
    1. Download and install version [16.4](https://usfirst.collab.net/sf/frs/do/listReleases/projects.wpilib/frs.frc_radio_configuration_utility) of the FRC Radio Configuration Utility
    1. Run the Utility located in "C:\Program Files (x86)\FRC Radio Configuration Utility\"
    1. Fill in the team number
    1. Set the appropriate radio type
    1. Leave the mode at "2.4GHz Access Point"
    1. Press the `Configure` button and exit the utility when finished
    1. Access the administrative interface on the radio at http://10.35.04.1/
    1. Login with username "admin" and a blank password
    1. Click `Wireless Settings` on the left side
    1. Add a robot name to the end of the network name to make it unique (e.g. "3504_PracticeBot")
    1. Click `Save Settings`
