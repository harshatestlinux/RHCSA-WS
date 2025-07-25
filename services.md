# System Services (systemd)

## Service Management
- `systemctl status [service]` - Show service status
- `systemctl start [service]` - Start a service
- `systemctl stop [service]` - Stop a service
- `systemctl restart [service]` - Restart a service
- `systemctl reload [service]` - Reload service configuration
- `systemctl enable [service]` - Enable service at boot
- `systemctl disable [service]` - Disable service at boot
- `systemctl mask [service]` - Mask a service (prevent start)
- `systemctl unmask [service]` - Unmask a service

## Service Information
- `systemctl list-units` - List all active units
- `systemctl list-units --type=service` - List all services
- `systemctl list-unit-files` - List all unit files
- `systemctl list-dependencies [service]` - Show service dependencies
- `systemctl show [service]` - Show service properties
- `systemctl cat [service]` - Show service unit file content

## System State
- `systemctl get-default` - Show default target
- `systemctl set-default multi-user.target` - Set default target
- `systemctl isolate rescue.target` - Switch to rescue mode
- `systemctl reboot` - Reboot system
- `systemctl poweroff` - Power off system
- `systemctl suspend` - Suspend system

## Targets (Runlevels)
- `poweroff.target` - Runlevel 0 (shutdown)
- `rescue.target` - Runlevel 1 (single-user mode)
- `multi-user.target` - Runlevel 3 (multi-user, no GUI)
- `graphical.target` - Runlevel 5 (multi-user with GUI)
- `reboot.target` - Runlevel 6 (reboot)

## Journal Logs
- `journalctl` - Show all journal entries
- `journalctl -u [service]` - Show logs for specific service
- `journalctl -f` - Follow journal in real-time
- `journalctl -b` - Show logs since last boot
- `journalctl --since "2023-01-01"` - Show logs since date
- `journalctl --until "2023-01-01"` - Show logs until date
- `journalctl -p err` - Show only error messages
- `journalctl --disk-usage` - Show journal disk usage

## Creating Custom Services
### Unit File Location
- `/etc/systemd/system/` - Custom unit files
- `/usr/lib/systemd/system/` - Package unit files

### Basic Unit File Structure
```ini
[Unit]
Description=My Custom Service
After=network.target

[Service]
Type=simple
User=myuser
ExecStart=/path/to/executable
Restart=always

[Install]
WantedBy=multi-user.target
```

## Timer Units (Cron alternative)
- `systemctl list-timers` - List all timers
- `systemctl enable --now mytimer.timer` - Enable and start timer

### Timer Unit Example
```ini
[Unit]
Description=My Timer
Requires=myservice.service

[Timer]
OnCalendar=daily
Persistent=true

[Install]
WantedBy=timers.target
```
