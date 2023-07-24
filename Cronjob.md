_**TO VIEW PRACTICAL, [CLICK HERE](#Practical)**_

# Cron jobs
`Cron` is a utility used in Unix systems to schedule tasks; it uses the cron daemon, `crond`, to run in the background and manage the execution of cron jobs by reading and processing the `cron table`. A cron job is a task scheduled to run in a given period that helps users automate routine tasks such as archiving logs or backing up system files. Cron jobs are maintained in cron tables, configuration files located at `/var/spool/cron/crontables` and '/etc/crontab' that define the schedules of tasks. Cron tables can be specific to the system, or individual users.

## Syntax
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
- The fifth asterisk represents the day of the week (0-7)

```
30 5 */2 * * /home/kayem/script.sh
```
The cron job above will execute /home/kayem/script.sh at 5:30 of every 2nd day of the month.

## Managing cron tables

There are two types of cron tables: system and user. System cron tables are located at `etc/crontab` and requires root privileges to edit, it contains scheduled tasks that apply to the entire system and tasks that need to be executed as the root for high-privileged operations. To edit the system cron table, you have to use a text editor such as vim, or nano, as attempting to edit it with `sudo contrab -e` will edit the personal cron tab of the root user, rather than the system cron table. As for the user cron table, each user has their own cron table. Although not created automatically, it can be created by a system administrator or by the user. An admin can create a cron table for a user with the command:
```
crontab -e -u [USER]
```
Users can also create their own cron table without needing any root privileges, but are limited to scheduling tasks with only the files they have permissions to execute. A user can create a cron table with the command:


# Practical

....
