## exportvg Command
### Purpose
Exports the definition of a volume group from a set of physical volumes.
### Syntax
exportvg VolumeGroup
### Description
The exportvg command removes the definition of the volume group specified by the VolumeGroup parameter from the system. Since all system knowledge of the volume group and its contents are removed, an exported volume group can no longer be accessed. The exportvg command does not modify any user data in the volume group.
A volume group is a nonshared resource within the system; it should not be accessed by another processor until it has been explicitly exported from its current processor and imported on another. The primary use of the exportvg command, coupled with the importvg command, is to allow portable volumes to be exchanged between processors. Only a complete volume group can be exported, not individual physical volumes.
Using the exportvg command and the importvg command, you can also switch ownership of data on physical volumes shared between two processors.
Note: To use this command, you must either have root user authority or be a member of the system group.
You can use the Volumes application in Web-based System Manager (wsm) to change volume characteristics.
You can use the Web-based System Manager Volumes application (wsm lvm fast path) to run this command. You could also use the System Management Interface Tool (SMIT) smit exportvg fast path to run this command.
Notes:
1.	A volume group that has a paging space volume on it cannot be exported while the paging space is active. Before exporting a volume group with an active paging space volume, ensure that the paging space is not activated automatically at system initialization, and then reboot the system.
2.	The mount point information of a logical volume would be missing from the LVCB (logical volume control block) if it is longer than 128 characters. Please make a note of the mount points that are longer than 128 characters as you will need to edit the /etc/filesystems file manually upon executingimportvg command to import this volume group completely.
### Exit Status
This command returns the following exit values:
Item	Description
0	Successful completion.
>0	An error occurred.
### Security
Attention RBAC users and Trusted AIX users: This command can perform privileged operations. Only privileged users can run privileged operations. For more information about authorizations and privileges, see Privileged Command Database in Security. For a list of privileges and the authorizations associated with this command, see the lssecattr command or the getcmdattr subcommand.
Examples
To remove volume group vg02 from the system, enter:
exportvg vg02
Note: The volume group must be varied off before exporting.
The definition of vg02 is removed from the system and the volume group cannot be accessed.
### Files
Item	Description
/usr/sbin	Directory where the exportvg command resides.
Parent topic: e

### Related information:
importvg command
varyonvg command
Logical volume storage
System management interface tool
 
