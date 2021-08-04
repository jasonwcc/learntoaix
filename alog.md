## alog Command
### Purpose
Creates and maintains fixed-size log files created from standard input.
### Syntax
To Show the Contents of a Log File
alog -f LogFile [ -o ] To Log Data to a Specified Log File
alog -f LogFile | [ [ -q ] [ -s Size ] ]
To Display the Verbosity Value of a Specified Log Type
alog -t LogType -V
To Change the Attributes of a Specified Log Type
alog -C -t LogType [ -f LogFile ] [ -s Size ] [ -w Verbosity ]
To Display the Current Attributes of a Specified Log Type
alog -L [ -t LogType ]
To Display the Usage of the alog Command
alog -H
### Description
The alog command reads standard input, writes to standard output, and copies the output into a fixed-size file. This file is treated as a circular log. If the file is full, new entries are written over the oldest existing entries.
The alog command works with log files that are specified on the command line or with logs that are defined in the alog configuration database. Logs that are defined in the alog configuration database are identified by LogType. The File, Size, and Verbosity attributes for each defined LogType are stored in the alog configuration database with the LogType. You can add a new LogType to the alog configuration database using the odmadd command. You can change the attributes of LogType defined in the alog configuration database using the alog command.
### Flags
Item	Description
-C	Changes the attributes for a specified LogType. Use the -C flag with the -f, -s, and -w flags to change the File, Size, andVerbosity attributes for the specified LogType. The -t LogType flag is required.
Note: Using the -C flag with -sSize only changes the size value in ODM and does not change the size of the actual log file.
If the -C flag is used, the alog command does not copy standard input to standard output or to a log file.
When the -C flag is used to modify the attributes for the console log type, the console log file is also modified and the console device driver is updated to use the new values. This is a deviation from the normal operation of alog -C and is done to accommodate special formatting in the console log file.
Note: You must have root user authority to change alog attributes.
-f LogFile	Specifies the name of a log file. If the specified log file does not exist, one is created. If the alog command is unable to write to a log file, it writes to /dev/null. Use the -f LogFile flag with the -C and -t flags to change the File attribute for a LogTypedefined in the alog configuration database.
-H	Displays the usage of the alog command.
-L	Lists the log types currently defined in the alog configuration database. If you use the -L flag with the -t LogType flag, the attributes for a specified LogType are listed. The current values of the File, Size, and Verbosity attributes are listed as colon separated values:
<File>:<Size>:<Verbosity>
If the -L flag is used, the alog command does not copy standard input to standard output or to File.
-o	Lists the contents of the log file. Writes the contents of the log file to standard output in sequential order.
-q	Copies standard input to a log file but does not write to standard output.
-s Size	Specifies the size limit of the log file in bytes. The space for the log file is reserved when it is created. If you create a new log file and do not specify the Size attribute, the minimum size, 4096 bytes, is used. If the log file already exists, its size will be changed. The size you specify is rounded upward to the next integral multiple of 4096 bytes. The maximum size for a log file is 2 GB. If the specified size is greater than 2 GB, only 2 GB is considered. If you decrease the size of the log file, the oldest entries in the log are deleted if they do not fit within the new size limit. You must have write permission for the log file to change its size.
Use the -s Size flag with the -C and the -t flags to change the Size attribute for LogType defined in the alog configuration database. Only the size value in ODM is changed. The size of the actual log file remains the same. The new Size attribute value is used the next time a log file is created.
-t LogType	Identifies a log defined in the alog configuration database. The alog command gets the log's file name and size from the alog configuration database. If LogFile does not exist, one is created.
If the alog command cannot get the information for the specified LogType from the alog configuration database or if the alogcommand is unable to write to LogFile, it writes to /dev/null.
If you specify LogType and LogFile using the -f flag, LogFile is used and LogType is ignored.
-V	Writes the current value of the Verbosity attribute for LogType that is defined in the alog configuration database to standard output. If you do not specify LogType, or the LogType you specify is not defined, nothing is written to standard output.
The value output using the alog command with the -t LogType and the -V flags can be used by a command that is piping its output to the alog command to control the verbosity of the data it writes to the pipe.
-w Verbosity	Changes the Verbosity attribute for LogType defined in the alog configuration database when used with the -C and the -tflags.
The Verbosity attribute can have a value from 0 to 9. If the value is 0, no information is copied to LogFile by the alogcommand. All of the information is still written to standard output. If the value is not 0, all of the information piped to thealog command's standard input is copied to LogFile and to standard output.

### Examples
1.	To record the current date and time in a log file named sample.log, enter:
date | alog -f /tmp/sample.log
2.	To list the contents of /tmp/sample.log log file, enter:
alog -f /tmp/sample.log -o
3.	To change the size of the log file named /tmp/sample.log to 8192 bytes, enter:
echo "resizing log file" | alog -f /tmp/sample.log -s 8192
4.	To add a new log type sample to the alog configuration database, create the alog.add file in the following format:
5.	SWservAt:
6.	    attribute="alog_type"
7.	    deflt="sample"
8.	    value="sample"
 
SWservAt:
    attribute="sample_logname"
    deflt="/tmp/sample.log"
    value="/tmp/sample.log"
 
SWservAt:
    attribute="sample_logsize"
    deflt="4096"
    value="4096"
 
SWservAt:
    attribute="sample_logverb"
    deflt="1"
    value="1"
After creating the alog.add file, enter:
odmadd alog.add
This adds the alog.add file to the SWservAt database.
9.	To change the name of the log file for the log type sample to /var/sample.log in the alog configuration database, enter:
alog -C -t sample -f /var/sample.log
10.	To change the size of the boot log to 8192 bytes and reflect the new size in ODM, enter:
11.	alog -C -t boot -s 8192
echo "Changed log size" | alog -t boot -s 8192

  ### Files
Item	Description
/etc/objrepos/SWservAt	Software Service Aids Attributes Object Class

 
