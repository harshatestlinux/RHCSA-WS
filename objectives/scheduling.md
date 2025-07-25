# Task Scheduling

## Cron Jobs
- `crontab -l` - List current user's cron jobs
- `crontab -e` - Edit current user's cron jobs
- `crontab -r` - Remove all cron jobs for current user
- `crontab -u username -l` - List cron jobs for specific user
- `crontab -u username -e` - Edit cron jobs for specific user

## Cron Format
```
# Minute Hour Day Month Weekday Command
# (0-59) (0-23) (1-31) (1-12) (0-7)
0 2 * * * /path/to/script.sh        # Daily at 2 AM
30 14 * * 1 /path/to/script.sh      # Every Monday at 2:30 PM
0 0 1 * * /path/to/script.sh        # First day of every month
*/15 * * * * /path/to/script.sh     # Every 15 minutes
0 9-17 * * 1-5 /path/to/script.sh   # Weekdays 9 AM to 5 PM
```

## System Cron Files
- `/etc/crontab` - System-wide cron table
- `/etc/cron.d/` - Additional cron files
- `/etc/cron.daily/` - Daily cron scripts
- `/etc/cron.weekly/` - Weekly cron scripts
- `/etc/cron.monthly/` - Monthly cron scripts
- `/etc/cron.hourly/` - Hourly cron scripts

## Cron Access Control
- `/etc/cron.allow` - Users allowed to use cron
- `/etc/cron.deny` - Users denied cron access
- If neither file exists, only root can use cron

## At Jobs (One-time scheduling)
- `at 15:30` - Schedule job for 3:30 PM today
- `at 15:30 tomorrow` - Schedule for 3:30 PM tomorrow
- `at 15:30 2023-12-25` - Schedule for specific date
- `at now + 5 minutes` - Schedule for 5 minutes from now
- `atq` - List pending at jobs
- `atrm job_number` - Remove at job
- `at -c job_number` - Show job details

## At Access Control
- `/etc/at.allow` - Users allowed to use at
- `/etc/at.deny` - Users denied at access

## Systemd Timers (Modern alternative to cron)
- `systemctl list-timers` - List all timers
- `systemctl list-timers --all` - List all timers including inactive
- `systemctl status timer.timer` - Show timer status
- `systemctl enable timer.timer` - Enable timer
- `systemctl start timer.timer` - Start timer

## Timer Unit Example
```ini
# /etc/systemd/system/backup.timer
[Unit]
Description=Daily backup timer
Requires=backup.service

[Timer]
OnCalendar=daily
Persistent=true

[Install]
WantedBy=timers.target
```

## Service Unit for Timer
```ini
# /etc/systemd/system/backup.service
[Unit]
Description=Daily backup service

[Service]
Type=oneshot
ExecStart=/usr/local/bin/backup.sh
```

## Timer Calendar Events
- `OnCalendar=daily` - Every day at midnight
- `OnCalendar=weekly` - Every Monday at midnight
- `OnCalendar=monthly` - First day of month at midnight
- `OnCalendar=*-*-* 02:00:00` - Every day at 2 AM
- `OnCalendar=Mon *-*-* 09:00:00` - Every Monday at 9 AM
- `OnCalendar=*-*-01 00:00:00` - First day of every month
