# AWS EC2 Bash Monitoring Assignment

This project is a two-day shell-scripting assignment completed on an Ubuntu AWS EC2 instance. It uses a Bash script to collect basic system information, saves the output to a log file, schedules the script with cron, and tracks the work with Git.

## What the monitoring script does

`monitor.sh` formats its output into clear sections and displays the following information:

| Section | Command used |
| --- | --- |
| Running services | `systemctl list-units --type=service --state=running` |
| CPU snapshot | `top -bn1 \| head -n 5` |
| Memory usage | `free -g` |
| Disk usage | `df -h` |

## Repository structure

```text
shell-assignment/
├── monitor.sh          # Bash monitoring script
├── monitor_logs.txt    # monitoring output
└── README.md           # Project documentation
```

## Setup

1. Launch an Ubuntu EC2 instance and connect to it securely with SSH. Use your own instance hostname and key file; do not commit or share the key.
2. Update packages and install Git:

   ```bash
   sudo apt update && sudo apt install git -y
   ```

3. Create and initialize the project directory:

   ```bash
   mkdir shell-assignment
   cd shell-assignment
   git init
   ```

4. Add `monitor.sh` to the directory and make it executable:

   ```bash
   chmod +x monitor.sh
   ```

## Run the monitor

Run the script manually to print the monitoring report in the terminal:

```bash
./monitor.sh
```

The report should include sections for services, CPU, memory, and disk space.

## Save monitoring output

Append each run to `monitor_logs.txt`:

```bash
./monitor.sh >> monitor_logs.txt
```

This keeps earlier reports in the same file instead of replacing them.

## Schedule the monitor with cron

Edit the current user's crontab:

```bash
crontab -e
```

The schedule captured for this project runs the script once per day and appends both standard output and errors to the log:

```cron
0 18 * * * /absolute/path/to/shell-assignment/monitor.sh >> /absolute/path/to/shell-assignment/monitor_logs.txt 2>&1
```

Important: `0 18 * * *` means **daily at 18:00 (6:00 PM) in the EC2 server's time zone**. Replace the placeholder paths with the actual absolute path to your project. Confirm the saved job with:

```bash
crontab -l
```

## Verify the assignment

Use these checks after setup:

```bash
./monitor.sh
tail -n 50 monitor_logs.txt
crontab -l
git log --oneline
git status
```

Expected results:

- The manual run prints all four monitoring sections.
- `monitor_logs.txt` contains appended reports.
- `crontab -l` shows the daily `0 18 * * *` entry.
- Git shows the commits made for the script, logging/cron update, and documentation.

### Memory-output note

The script uses `free -g`, which reports memory in whole gibibytes. On a small EC2 instance with less than 1 GiB of RAM, several values can display as `0` because of coarse rounding. This does not mean the instance has no memory; the `top` output still shows memory values in MiB.

## Version control

After making changes, add, commit, and push them to your GitHub repository:

```bash
git branch -M main
git remote add origin "https://github.com/adhil6407/task-1.git"
git push -u origin main
```
