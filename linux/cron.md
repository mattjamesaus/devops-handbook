# Cron

### Stagged Scheduling
It's often helpful to add jitter to any form of cronjob you're setting up, adding jitter will help to protect against a number of things like:
- resource exhaustion on downstream dependencies
- resource exhaustion on the host due to crons overlapping or triggering at t=0
- if they're running in kubernetes with cron -f you don't make nodes hosting said pods all hot at once leading to un-necessary scaleups.

#### Resources
https://www.endpointdev.com/blog/2020/06/randomly-spacing-cron-jobs/

### Resource Limits
Set resource limits (CPU, memory) for your cron jobs to prevent them from consuming excessive resources and impacting other services. On a Linux system, you can use cgroups to limit resources. For example, create a control group and assign limits:

```sh
cgcreate -g memory,cpu:cronjobs
echo 100M > /sys/fs/cgroup/memory/cronjobs/memory.limit_in_bytes
echo 50% > /sys/fs/cgroup/cpu/cronjobs/cpu.cfs_quota_us
```
Then, run your cron job within this cgroup:

```sh
cgexec -g memory,cpu:cronjobs /path/to/your/cronjob.sh
```

### Dependency Management
Ensure that cron jobs are independent of each other wherever possible. If dependencies are necessary, implement robust error handling and retries to manage failures gracefully without cascading effects. Use tools like wait-for-it to ensure dependent services are up, and add retry logic within your scripts:

```sh
RETRIES=5
for i in $(seq 1 $RETRIES); do
    /path/to/dependent-service-check && break || sleep 5
    if [ $i -eq $RETRIES ]; then
        echo "Dependency check failed after $RETRIES attempts" >&2
        exit 1
    fi
done
```
### Overlapping Jobs
If a cron job takes longer to execute than the interval at which it's scheduled, multiple instances can overlap, leading to resource contention and potential failures. To prevent this, use a lock file or a job management tool like flock:

```sh
* * * * * flock -n /tmp/your_job.lock /path/to/your/script.sh
```
Or, implement a locking mechanism within your script:

```sh
#!/bin/bash
LOCKFILE=/tmp/your_job.lock
if [ -e $LOCKFILE ]; then
    echo "Job already running"
    exit 1
fi
touch $LOCKFILE
trap "rm -f $LOCKFILE" EXIT
# Your script logic here
```

## Gotchas
### Misinterpreted Special Characters
Certain special characters in cron job definitions can cause unexpected behavior. For example, using % in the command without proper escaping can lead to issues as cron treats it as a newline character. To avoid this, always escape % with a backslash (\%).

### Environment Variables
Cron jobs do not inherit the same environment as your user shell. This can lead to problems if your job relies on specific environment variables. Always define necessary variables within your script or in the crontab itself:
```sh
* * * * * PATH=/usr/local/bin:/usr/bin:/bin; /path/to/your/script.sh
```
### Time Zone Differences
Cron uses the system's time zone by default, which may differ from your expected time zone. Ensure that your cron jobs account for time zone differences or explicitly set the time zone within your script:

```sh
#!/bin/bash
export TZ=Australia/Perth
date
/path/to/your/script.sh
```

### Silent Failures
Cron jobs often fail silently, making it difficult to identify and debug issues. Always redirect the output (both stdout and stderr) to log files:
```sh
* * * * * /path/to/your/script.sh >> /path/to/your/logfile.log 2>&1
```
Consider setting up email notifications for cron job failures by adding MAILTO in your crontab:
```sh
MAILTO=your_email@example.com
* * * * * /path/to/your/script.sh
```
### Incorrect Syntax
A common mistake is incorrect crontab syntax, especially with the five time fields. Use an online cron expression generator to ensure accuracy. Additionally, remember to use the correct user for the crontab:
```sh
sudo crontab -e -u your_user
```
Limited Shell
Cron uses a limited shell (usually /bin/sh). If your script requires a more advanced shell (e.g., bash), specify it at the beginning of your script:

```sh
#!/bin/bash
# Your script logic here
```
