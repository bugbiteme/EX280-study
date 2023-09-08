# cron jobs in OpenShift

- Create a cron job `say-hello` in a project `cron-test` called that runs at 4:05 am every Sunday
- cron job runs in a pod created from the image `quay.io/redhattraining/hello-world-nginx`
- cron job runs the command `echo Hello from the OpenShift cluster`
  
1. Create the `cron-test` project  
2. From the project, in Administrator view, go to `Workloads->CronJobs`
3. Click `[Create Cronjob]`
4. Fill out the yaml with the following config
```
apiVersion: batch/v1
kind: CronJob
metadata:
  name: say-hello
  namespace: cron-test
spec:
  schedule: '5 4 * * 0'
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: hello
              image: quay.io/redhattraining/hello-world-nginx
              args:
                - /bin/sh
                - '-c'
                - date; echo Hello from the OpenShift cluster
          restartPolicy: OnFailure

```
Once deployed, run the command oc get all to see the job in the project. Note you can change the schedule to see it run more often. `*/1 * * * *` will make it run every minute and look at the pod logs for the message.



  [back to main](./README.md) 