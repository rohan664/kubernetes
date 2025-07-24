# CronJob and Job in kubernetes

## Cronjob
CronJob is meant for performing regular scheduled actions such as backups, report generation, and so on. One CronJob object is like one line of a crontab (cron table) file on a Unix system. It runs a Job periodically on a given schedule, written in Cron format.

### Cronjob syntax
``` # ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of the month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday)
# │ │ │ │ │       
# │ │ │ │ │
# │ │ │ │ │
```

### spec.startingDeadlineSeconds
The startingDeadlineSeconds field sets a strict time limit for a CronJob to start after its scheduled time. If the job fails to begin within this window (e.g., due to high cluster load or controller delays), Kubernetes marks it as failed and skips execution.

Example: Cache Cleanup CronJob
Suppose you schedule a job to clean up cache at 3 AM daily with a 15-minute deadline (startingDeadlineSeconds: 900).

Scenario 1: Job Starts On Time
Cluster is healthy → Job launches at 3:00 AM and succeeds.

Scenario 2: Cluster Overloaded (Delayed Start) 
Job starts at 3:10 AM (within 15-minute deadline) → Runs normally.

Scenario 3: Critical Delay (Deadline Exceeded) Job fails to start by 3:15 AM → Kubernetes:Marks it as failed.Skips execution entirely.Logs a Missed Schedule event.

### 
The .spec.concurrencyPolicy field is also optional. It specifies how to treat concurrent executions of a Job that is created by this CronJob. The spec may specify only one of the following concurrency policies:

 - Allow (default): The CronJob allows concurrently running Jobs

 - Forbid: The CronJob does not allow concurrent runs; if it is time for a new Job run and the previous Job run hasn't finished yet, the CronJob skips the new Job run. Also note that when the previous Job run finishes, .spec.startingDeadlineSeconds is still taken into account and may result in a new Job run.

 - Replace: If it is time for a new Job run and the previous Job run hasn't finished yet, the CronJob replaces the currently running Job run with a new Job run
Note that concurrency policy only applies to the Jobs created by the same CronJob. If there are multiple CronJobs, their respective Jobs are always allowed to run concurrently.

### Timezone
After version v1.27 kubernetes support timezone specification under spec.timezone field. By default, it set to UTC. This timezone specification will impact on schedule time not on container timezone.