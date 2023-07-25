_**TO VIEW PRACTICAL, [CLICK HERE](#Practical)**_

# Cron jobs
`Cron` is a utility used in Unix systems to schedule tasks; it uses the cron daemon, `crond`, to run in the background and manage the execution of cron jobs by reading and processing the `cron table`. A cron job is a task scheduled to run in a given period that helps users automate routine tasks such as archiving logs or backing up system files. Cron jobs are maintained in cron tables, configuration files located at `/var/spool/cron/crontabs` and '/etc/crontab' that define the schedules of tasks. Cron tables can be specific to the system or individual users.

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
- The fifth asterisk represents the day of the week (0-7, where 0 and 7 both represent Sunday)

Cron syntax also uses operators on provided values in cron fields.
- An asterisk (*) represents all values.
- A comma (,) specifies a list of values.
- A dash (-) indicates a range of values.
- A forward slash (/) is used as a step value.

```
30 5 */2 * * /home/kayem/script.sh
```
The cron job above will execute /home/kayem/script.sh at 5:30 on every second day of the month.

## Managing cron tables

There are two types of cron tables: system and user. System cron tables are located at `etc/crontab` and require root privileges to edit. They contain scheduled tasks that apply to the entire system and tasks that need to be executed as the root for high-privileged operations. To edit the system cron table, you have to use a text editor such as Vim, or Nano, as attempting to edit it with `sudo cron tab -e` will revise the personal cron table of the root user, rather than the system cron table. As for the user cron table, each user has their own cron table. Although it is not created automatically, it can be created by a system administrator or the user. An admin can create a cron table for a user with the command:
```
crontab -e -u [USER]
```
Users can create a cron table without root privileges but are limited to scheduling tasks with the files they have permission to execute.

## Security implications of cron jobs

If misconfigured, cron jobs can exploit high privileges and gain unauthorised access to critical system resources. Therefore, it is necessary that personal cron jobs run with the privileges of the user; rather than higher privileged users. If a cron job file has the SUID bit set, is owned by a higher privileged user, like root, and a regular user has write permissions, they can inject malicious code to be executed that spawns a root shell, deletes critical files, terminates services, and the like.

# Practical

....
