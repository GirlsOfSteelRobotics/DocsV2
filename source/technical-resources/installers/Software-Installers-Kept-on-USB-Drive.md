# Software Installers Kept on USB Drives

## Required Installers

| Name | Version | Mac? | Win? | URL |
| :--- | :------ | :--- | ---- | :-- |
| CTRE Phoenix Framework | 5.17.4.0 | Y | Y | [CTRE Hero Tech Resources](http://www.ctr-electronics.com/control-system/hro.html#product_tabs_technical_resources) |
| CTRE Talon SRX Firmware | 20.0 | n/a | n/a | _Installed by the Phoenix package above to_<br>C:\Users\Public\Public Documents\FRC\ |
| CTRE-Installer | 1.0 | Y | no | [CTRE Installer from this repository](https://github.com/GirlsOfSteelRobotics/Docs/blob/master/CTRE-Installer.command) |
| FRC Radio Config Util | 20.0.0 | no | Y | [Radio config docs](https://docs.wpilib.org/en/latest/docs/getting-started/getting-started-frc-control-system/radio-programming.html) |
| GIT-SCM | 2.24.1.2 | no | Y | [GIT-SCM Downloads](https://git-scm.com/download/) |
| GRIP | 1.5.2 | Y | Y | [GRIP releases](https://github.com/WPIRoboticsProjects/GRIP/releases) |
| Limelight Image Tool | 2020.2 | Y | no | [Limelight Etcher Flash Tool, USB Flash Driver & Image](https://limelightvision.io/pages/downloads) |
| NI FRC Update Suite | 2020.? | no | Y | [NI Download Site](https://www.ni.com/en-us/support/downloads/drivers/download.frc-game-tools.html) (Use the **Individual Offline Installers** link) |
| Rev Robotics Color Sensor v3 Java Library | ? | Y | Y | Command/Ctrl-Shift-P _WPILib: Install new library (Online)_ from `http://revrobotics.com/content/sw/color-sensor-v3/sdk/REVColorSensorV3.json` |
| Spark Max Client App | 2.1.1 (with fw 1.5.2) | no | Y | [Spark Max Software](https://www.revrobotics.com/sparkmax-software/#spark-max-client-application) |
| Spark Max Java Library | 1.5.1 | Y | Y | Command/Ctrl-Shift-P _WPILib: Install new library (Online)_ from `https://www.revrobotics.com/content/sw/max/sdk/REVRobotics.json` |
| Visual Studio Code | 1.41.1 | Y | no | Mac only! [VS Code Downloads](https://code.visualstudio.com/download) |
| WPILib | 2020.3.2 | Y | Y | [WPILib Releases](https://github.com/wpilibsuite/allwpilib/releases) |
| WPILib-Installer | 1.0 | Y | no | [WPILib Installer from this repository](https://github.com/GirlsOfSteelRobotics/Docs/blob/master/WPILib-Installer.command) |


## Helpful Commands

The rsync command is a fast way copy files, skipping any files that are already up to date at the destination. This command will update a USB drive mounted on /Volumes/FRC from an updated folder at ~/Desktop/FRC:

`rsync --itemize-changes --recursive --delete --times --omit-dir-times --modify-window=1 --specials --links --exclude=.DS_Store --exclude=._.DS_Store --exclude=/.Spotlight-V100 --exclude=.fseventsd --exclude='/System Volume Information' --exclude=.Trashes --exclude=.TemporaryItems ~/Desktop/FRC/ /Volumes/FRC`
