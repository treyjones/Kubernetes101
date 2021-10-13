### CRONJOBS
=> CronJobs are meant for performing regular scheduled actions such as backups, report generation, and so on. Each of those tasks should be configured to recur indefinitely (for example: once a day / week / month); you can define the point in time within that interval when the job should start

### CRONJOB SCHEDULE SYNTAX
```sh
#      ┌────────────────── timezone (optional)
#      |      ┌───────────── minute (0 - 59)
#      |      │ ┌───────────── hour (0 - 23)
#      |      │ │ ┌───────────── day of the month (1 - 31)
#      |      │ │ │ ┌───────────── month (1 - 12)
#      |      │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday;
#      |      │ │ │ │ │                                   7 is also Sunday on some systems)
#      |      │ │ │ │ │
#      |      │ │ │ │ │
# CRON_TZ=UTC * * * * *

```
> For every CronJob, the CronJob Controller checks how many schedules it missed in the duration from its last scheduled time until now. If there are more than 100 missed schedules, then it does not start the job and logs the error

### Jobs
=> A Job creates one or more Pods and will continue to retry execution of the Pods until a specified number of them successfully terminate. As pods successfully complete, the Job tracks the successful completions. When a specified number of successful completions is reached, the task (ie, Job) is complete. Deleting a Job will clean up the Pods it created. Suspending a Job will delete its active Pods until the Job is resumed again.

#### Parallel execution for Jobs
There are three main types of tasks suitable to run as a Job:

##### 1. Non-parallel Jobs
- Only one Pod is started, unless the Pod fails.
the Job is complete as soon as its Pod terminates successfully.
##### 2. Parallel Jobs with a fixed completion count:
specify a non-zero positive value for ```sh .spec.completions```.
-The Job represents the overall task, and is complete when there are .spec.completions successful Pods.
when using ```sh .spec.completionMode="Indexed" ```, each Pod gets a different index in the range 0 to .spec.completions-1.
##### 3. Parallel Jobs with a work queue:
do not specify ```sh .spec.completions```, default to ```sh.spec.parallelism```.
-The Pods must coordinate amongst themselves or an external service to determine what each should work on. 
> For example, a Pod might fetch a batch of up to N items from the work queue.
- each Pod is independently capable of determining whether or not all its peers are done, and thus that the entire Job is done.
- when any Pod from the Job terminates with success, no new Pods are created.
once at least one Pod has terminated with success and all Pods are terminated, then the Job is completed with success.
- once any Pod has exited with success, no other Pod should still be doing any work for this task or writing any output. They should all be in the process of exiting.

### Handling failures
> A container in a Pod may fail for a number of reasons, such as because the process in it exited with a non-zero exit code, or the container was killed for exceeding a memory limit, etc. If this happens, and the ```sh .spec.template.spec.restartPolicy = "OnFailure" ```, then the Pod stays on the node, but the container is re-run. Therefore, your program needs to handle the case when it is restarted locally, or else specify ```sh .spec.template.spec.restartPolicy = "Never" ```.

##### Output yaml config of a resource
```sh
kubectl get job myjob -o yaml
```

### Replication Controller 
Jobs are complementary to Replication Controllers. A Replication Controller manages Pods which are not expected to terminate (e.g. web servers), and a Job manages Pods that are expected to terminate (e.g. batch tasks).

As discussed in Pod Lifecycle, Job is only appropriate for pods with RestartPolicy equal to OnFailure or Never. **(Note: If RestartPolicy is not set, the default value is Always)**