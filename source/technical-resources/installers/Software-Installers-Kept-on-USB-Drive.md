# Software Installers Kept on USB Drives

## Required Installers

| Name | Version | Mac? | Win? | URL |
| :--- | :------ | :--- | ---- | :-- |
| CTRE Phoenix Framework | 5.21.1 | no | Y | [CTRE Phoenix Framework](https://store.ctr-electronics.com/software/) |
| CTRE Talon SRX Firmware | 22.0 | n/a | n/a | _Installed by the Phoenix package above to_<br>C:\Users\Public\Public Documents\FRC\ |
| FRC Radio Config Util | 22.0.1 | no | Y | [Installer ZIP file](https://firstfrc.blob.core.windows.net/frc2022/Radio/FRC_Radio_Configuration_22_0_1.zip) |
| npcap Library | 1.60 | no | Y | Install after the above [Download](https://npcap.com/#download)|
| GIT-SCM | 2.35.1.2 | no | Y | [GIT-SCM Downloads](https://git-scm.com/download/) |
| GRIP | 1.5.2 | Y | Y | [GRIP releases](https://github.com/WPIRoboticsProjects/GRIP/releases) |
| Limelight Image Tool | 2022.1 | Y | no | [Limelight Etcher Flash Tool, USB Flash Driver & Image](https://limelightvision.io/pages/downloads) |
| NI FRC Update Suite | 2022 f1 | no | Y | [NI Download Site](https://www.ni.com/en-us/support/downloads/drivers/download.frc-game-tools.html#) (Use the **Individual Offline Installers** link) |
| REV Hardware Client | 1.4.2 | no | Y | [Getting Started with the REV Hardware Client](https://docs.revrobotics.com/sparkmax/rev-hardware-client/getting-started-with-the-rev-hardware-client) |
| WPILib | 2022.3.1 | Y | Y | [WPILib Releases](https://github.com/wpilibsuite/allwpilib/releases/) |

## Helpful Commands

The rsync command is a fast way copy files, skipping any files that are already up to date at the destination. This command will update a USB drive mounted on /Volumes/FRC from an updated folder at ~/Desktop/FRC:

`rsync --itemize-changes --recursive --delete --times --omit-dir-times --modify-window=1 --specials --links --exclude=.DS_Store --exclude=._.DS_Store --exclude=/.Spotlight-V100 --exclude=.fseventsd --exclude='/System Volume Information' --exclude=.Trashes --exclude=.TemporaryItems ~/Desktop/FRC/ /Volumes/FRC`
