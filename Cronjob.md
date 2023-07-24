_**TO VIEW PRACTICAL, [CLICK HERE](#Practical)**_

# Cron jobs
`Cron` is a utility used in Unix systems to schedule tasks; it uses the cron daemon, `crond`, to run in the background and manage the execution of cron jobs by reading and processing the `cron table`. A cron job is a task scheduled to run in a given period that helps users automate routine tasks such as archiving logs or backing up system files. Cron jobs are maintained in the cron table, a configuration file located at `/var/spool/cron/crontables` that defines the schedule. Cron tables can assign system or user cron jobs.

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
- the first asterisk represents the minute (0-59)
- the second asterisk represents the hour (0-23)
- the third asterisk represents the day of the month (1-31)
- the fourth asterisk represents the month (1-12)
- the fifth asterisk represents the day of the week (0-7)

# Practical

....
