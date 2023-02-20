
# CRONTAB

INFO: https://crontab.guru/crontab.5.html

## IMPORTANTE: EXPLICARLO!


### DOCUMENTACIÓN PROVISIONAL
CRONTAB
Section: File Formats (5)
Updated: 2012-11-22
 
NAME
crontab - files used to schedule the execution of programs  
DESCRIPTION
A crontab file contains instructions for the cron(8) daemon in the following simplified manner: "run this command at this time on this date". Each user can define their own crontab. Commands defined in any given crontab are executed under the user who owns that particular crontab. Uucp and News usually have their own crontabs, eliminating the need for explicitly running su(1) as part of a cron command.

Blank lines, leading spaces, and tabs are ignored. Lines whose first non-white space character is a pound-sign (#) are comments, and are not processed. Note that comments are not allowed on the same line as cron commands, since they are considered a part of the command. Similarly, comments are not allowed on the same line as environment variable settings.

An active line in a crontab is either an environment setting or a cron command. An environment setting is of the form:


   name = value

where the white spaces around the equal-sign (=) are optional, and any subsequent non-leading white spaces in value is a part of the value assigned to name. The value string may be placed in quotes (single or double, but matching) to preserve leading or trailing white spaces.

Several environment variables are set up automatically by the cron(8) daemon. SHELL is set to /bin/sh, and LOGNAME and HOME are set from the /etc/passwd line of the crontab's owner. HOME and SHELL can be overridden by settings in the crontab; LOGNAME can not.

(Note: the LOGNAME variable is sometimes called USER on BSD systems and is also automatically set).

In addition to LOGNAME, HOME, and SHELL, cron(8) looks at the MAILTO variable if a mail needs to be send as a result of running any commands in that particular crontab. If MAILTO is defined (and non-empty), mail is sent to the specified address. If MAILTO is defined but empty (MAILTO=), no mail is sent. Otherwise, mail is sent to the owner of the crontab. This option is useful if you decide to use /bin/mail instead of /usr/lib/sendmail as your mailer. Note that /bin/mail does not provide aliasing and UUCP usually does not read its mail. If MAILFROM is defined (and non-empty), it is used as the envelope sender address, otherwise, ``root'' is used.

By default, cron sends a mail using the 'Content-Type:' header of 'text/plain' with the 'charset=' parameter set to the 'charmap/codeset' of the locale in which crond(8) is started up, i.e., either the default system locale, if no LC_* environment variables are set, or the locale specified by the LC_* environment variables (see locale(7)). Different character encodings can be used for mailing cron job outputs by setting the CONTENT_TYPE and CONTENT_TRANSFER_ENCODING variables in a crontab to the correct values of the mail headers of those names.

The CRON_TZ variable specifies the time zone specific for the cron table. The user should enter a time according to the specified time zone into the table. The time used for writing into a log file is taken from the local time zone, where the daemon is running.

The MLS_LEVEL environment variable provides support for multiple per-job SELinux security contexts in the same crontab. By default, cron jobs execute with the default SELinux security context of the user that created the crontab file. When using multiple security levels and roles, this may not be sufficient, because the same user may be running in different roles or in different security levels. For more information about roles and SELinux MLS/MCS, see selinux(8) and the crontab example mentioned later on in this text. You can set the MLS_LEVEL variable to the SELinux security context string specifying the particular SELinux security context in which you want jobs to be run. crond will then set the execution context of those jobs that meet the specifications of the particular security context. For more information, see crontab(1) -s option.

The RANDOM_DELAY variable allows delaying job startups by random amount of minutes with upper limit specified by the variable. The random scaling factor is determined during the cron daemon startup so it remains constant for the whole run time of the daemon.

The format of a cron command is similar to the V7 standard, with a number of upward-compatible extensions. Each line has five time-and-date fields followed by a username (if this is the system crontab file), and followed by a command. Commands are executed by cron(8) when the 'minute', 'hour', and 'month of the year' fields match the current time, and at least one of the two 'day' fields ('day of month', or 'day of week') match the current time (see "Note" below).

Note that this means that non-existent times, such as the "missing hours" during the daylight savings time conversion, will never match, causing jobs scheduled during the "missing times" not to be run. Similarly, times that occur more than once (again, during the daylight savings time conversion) will cause matching jobs to be run twice.

cron(8) examines cron entries every minute.

The time and date fields are:

    field	allowed values
    minute	0-59
    hour	0-23
    day of month	1-31
    month	1-12 (or names, see below)
    day of week	0-7 (0 or 7 is Sunday, or use names)

A field may contain an asterisk (*), which always stands for "first-last".

Ranges of numbers are allowed. Ranges are two numbers separated with a hyphen. The specified range is inclusive. For example, 8-11 for an 'hours' entry specifies execution at hours 8, 9, 10, and 11.

Lists are allowed. A list is a set of numbers (or ranges) separated by commas. Examples: "1,2,5,9", "0-4,8-12".

Step values can be used in conjunction with ranges. Following a range with "/<number>" specifies skips of the number's value through the range. For example, "0-23/2" can be used in the 'hours' field to specify command execution for every other hour (the alternative in the V7 standard is "0,:2,:4,:6,:8,:10,:12,:14,:16,:18,:20,:22"). Step values are also permitted after an asterisk, so if specifying a job to be run every two hours, you can use "*/2".

Names can also be used for the 'month' and 'day of week' fields. Use the first three letters of the particular day or month (case does not matter). Ranges or lists of names are not allowed.

The "sixth" field (the rest of the line) specifies the command to be run. The entire command portion of the line, up to a newline or a "%" character, will be executed by /bin/sh or by the shell specified in the SHELL variable of the cronfile. A "%" character in the command, unless escaped with a backslash (\), will be changed into newline characters, and all data after the first % will be sent to the command as standard input.

Note: The day of a command's execution can be specified in the following two fields --- 'day of month', and 'day of week'. If both fields are restricted (i.e., do not contain the "*" character), the command will be run when either field matches the current time. For example,
"30 4 1,15 * 5" would cause a command to be run at 4:30 am on the 1st and 15th of each month, plus every Friday.  
EXAMPLE CRON FILE

# use /bin/sh to run commands, no matter what /etc/passwd says
SHELL=/bin/sh
# mail any output to `paul', no matter whose crontab this is
MAILTO=paul
#
CRON_TZ=Japan
# run five minutes after midnight, every day
5 0 * * *       $HOME/bin/daily.job >> $HOME/tmp/out 2>&1
# run at 2:15pm on the first of every month -- output mailed to paul
15 14 1 * *     $HOME/bin/monthly
# run at 10 pm on weekdays, annoy Joe
0 22 * * 1-5    mail -s "It's 10pm" joe%Joe,%%Where are your kids?%
23 0-23/2 * * * echo "run 23 minutes after midn, 2am, 4am ..., everyday"
5 4 * * sun     echo "run at 5 after 4 every sunday"

 
Jobs in /etc/cron.d/
The jobs in cron.d and /etc/crontab are system jobs, which are used usually for more than one user, thus, additionally the username is needed. MAILTO on the first line is optional.  
EXAMPLE OF A JOB IN /etc/cron.d/job

#login as root
#create job with preferred editor (e.g. vim)
MAILTO=root
* * * * * root touch /tmp/file

 
SELinux with multi level security (MLS)
In a crontab, it is important to specify a security level by crontab -s or specifying the required level on the first line of the crontab. Each level is specified in /etc/selinux/targeted/seusers. When using crontab in the MLS mode, it is especially important to:
- check/change the actual role,
- set correct role for directory, which is used for input/output.  
EXAMPLE FOR SELINUX MLS

# login as root
newrole -r sysadm_r
mkdir /tmp/SystemHigh
chcon -l SystemHigh /tmp/SystemHigh
crontab -e
# write in crontab file
MLS_LEVEL=SystemHigh
0-59 * * * * id -Z > /tmp/SystemHigh/crontest

 
FILES
/etc/crontab main system crontab file. /var/spool/cron/ a directory for storing crontabs defined by users. /etc/cron.d/ a directory for storing system crontabs.  
SEE ALSO
cron(8), crontab(1)  
EXTENSIONS
These special time specification "nicknames" which replace the 5 initial time and date fields, and are prefixed with the '@' character, are supported:

@reboot    :    Run once after reboot.
@yearly    :    Run once a year, ie.  "0 0 1 1 *".
@annually  :    Run once a year, ie.  "0 0 1 1 *".
@monthly   :    Run once a month, ie. "0 0 1 * *".
@weekly    :    Run once a week, ie.  "0 0 * * 0".
@daily     :    Run once a day, ie.   "0 0 * * *".
@hourly    :    Run once an hour, ie. "0 * * * *".

 
CAVEATS
crontab files have to be regular files or symlinks to regular files, they must not be executable or writable for anyone else but the owner. This requirement can be overridden by using the -p option on the crond command line. If inotify support is in use, changes in the symlinked crontabs are not automatically noticed by the cron daemon. The cron daemon must receive a SIGHUP signal to reload the crontabs. This is a limitation of the inotify API.  
AUTHOR
Paul Vixie
examples tips man page cron reference cron monitoring uptime monitoring status 