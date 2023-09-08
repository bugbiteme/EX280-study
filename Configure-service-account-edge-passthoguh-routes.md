#  Configure service account

## creating a service account with scc to allow app to run

- log in as developer

- create new project `appsec-scc`  
 

- deploy an app without exposing a route
`oc new-app --name nginx --image nginx`   

- note that the application fails to start  

```
❯ oc get pods                           
NAME                      READY   STATUS             RESTARTS      AGE
nginx-69567d67f8-nwvp2   0/1     CrashLoopBackOff   2 (21s ago)   42s
```
- check logs pod logs
```
❯ oc logs pod/nginx-69567d67f8-nwvp2 
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
.
.
.
nginx: [warn] the "user" directive makes sense only if the master process runs with super-user privileges, ignored in /etc/nginx/nginx.conf:2
2023/09/07 23:19:46 [emerg] 1#1: mkdir() "/var/cache/nginx/client_temp" failed (13: Permission denied)
nginx: [emerg] mkdir() "/var/cache/nginx/client_temp" failed (13: Permission denied)
```  
- we need to create a service account and assign the anyuid SCC to it to get the app to run
- login as `admin`  

- create the `nginx-sa` service account  

- assign the `anyuid` SCC to nginx-sa  

- log back in as `developer` (or `leader`)
  
- assign the service account to the deployment


This will cause the app to re-deploy and the app should run now
```
❯ oc get pods                                      
NAME                      READY   STATUS    RESTARTS   AGE
nginx-58c5496cd8-69s58   1/1     Running   0          87s
```

- expose the service

## create an edge route (https)
  
- create a secure route (edge)  


## create a tls passthrough route

- create a tls secret

- create passthrough route

  
  
  [back to main](./README.md) 