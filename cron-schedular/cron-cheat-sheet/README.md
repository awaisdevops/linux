# Cron Scheduler Guide

This document provides an overview of the cron, anacron, and at schedulers in Linux. It covers how to schedule jobs using these schedulers, manage their services, and configure tasks to run at specific times or intervals.

---

## Types of Schedulers

1. **Crontab Scheduler**  
2. **Anacrontab Scheduler**  
3. **At Scheduler**  

### 1. Crontab Scheduler

Cron is a task scheduler in Linux used to schedule jobs that need to be executed periodically. A job can be a script, a backup, or any other task you need to run at a specific time.

#### How Cron Works

- The `cron` service is always running, even if no jobs are scheduled.
- It checks the crontab every minute to see if any job matches the current time.
- If a match is found, the job is executed; otherwise, it waits until the next minute.
- The cron service operates from the `/var/spool/cron/` directory.

#### Checking Cron Service Status

```bash
systemctl status crond.service
systemctl is-active crond.service
```

#### Editing Cron Jobs

To edit cron jobs for the currently logged-in user:

```bash
crontab -e
```

To edit cron jobs for a specific user:

```bash
crontab -u username -e
```

#### Cron Job Format

Cron jobs are written in the following format:

```bash
* * * * * <command_to_run>
| | | | |
| | | | +---- Day of the week (0 - 7) (Sunday = 0 or 7)
| | | +------ Month (1 - 12)
| | +-------- Day of the month (1 - 31)
| +---------- Hour (0 - 23)
+------------ Minute (0 - 59)
```

#### Cron Job Examples

- Run a backup every day at 2:15 AM:

  ```bash
  15 2 * * * cp /opt/db /tmp/db_backup
  ```

- Run a job every Wednesday:

  ```bash
  * * * * 3
  ```

- Run a job every 18th day of the month at 9:30 PM:

  ```bash
  30 21 18 * 2
  ```

- Run a job every New Year's Day:

  ```bash
  00 00 01 01 *
  ```

- Run a job every minute:

  ```bash
  1 * * * *
  ```

- Run a job every hour:

  ```bash
  * 1 * * *
  ```

#### Important Tools

To calculate and verify cron schedules, use [crontab guru](https://crontab.guru/).

- List all cron jobs for the current user:

  ```bash
  crontab -l
  ```

- Cron jobs are stored in:

  ```bash
  /var/spool/cron/root
  ```

- To backup the cron jobs:

  ```bash
  cp /var/spool/cron/root /tmp/
  ```

- Cron job logs are stored in:

  ```bash
  /var/log/cron
  ```

#### User Restrictions

A standard user can create their own cron jobs, but to restrict access, you can deny them by editing the cron deny file:

```bash
vim /etc/cron.deny
```

Add the username to restrict that user from creating cron jobs.

---

### 2. Anacrontab Scheduler

Anacron is used to execute jobs that were missed by `cron` (due to power failure or system downtime). It ensures that tasks are executed even if the system was down when they were supposed to run.

#### Anacron Configuration

The anacron jobs are configured in the `/etc/anacrontab` file.

---

### 3. At Scheduler

The `at` scheduler is used to schedule a one-time task to be executed at a specific time. Unlike cron, the at scheduler is not for recurring tasks.

#### Scheduling Jobs with `at`

To schedule a job to run at 6 PM:

```bash
at -f /opt/db-back.sh 6pm
```

#### Managing the At Service

- Check the status of the `atd` service:

  ```bash
  systemctl status atd
  ```

- Restart the `atd` service:

  ```bash
  systemctl restart atd
  ```

- Enable the `atd` service to start on boot:

  ```bash
  systemctl enable atd
  ```

- Check if the `atd` service is active:

  ```bash
  systemctl is-active atd
  ```

---

### Additional Notes

- Cron jobs are useful for repetitive tasks such as backups, maintenance, and log rotation.
- The `at` scheduler is ideal for one-time tasks like system reboots, specific jobs, or temporary actions.
- Anacron ensures that missed jobs are executed after an outage, ensuring important tasks are not missed.

---

This guide serves as an introduction to the three main Linux schedulers: cron, anacron, and at. They are essential tools for automating tasks, managing backups, and ensuring system maintenance tasks are performed regularly.
