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
```
❯ oc create sa nginx-sa                                                                               
serviceaccount/nginx-sa created
```  

- assign the `anyuid` SCC to nginx-sa  
```
❯ oc adm policy add-scc-to-user anyuid -z nginx-sa   
clusterrole.rbac.authorization.k8s.io/system:openshift:scc:anyuid added: "nginx-sa"
```  

- log back in as `developer` (or `leader`)
  
- assign the service account to the deployment
```
❯ oc set serviceaccount deployment/nginx nginx-sa
deployment.apps/nginx serviceaccount updated
```  
  
This will cause the app to re-deploy and the app should run now
```
❯ oc get pods                                      
NAME                      READY   STATUS    RESTARTS   AGE
nginx-58c5496cd8-69s58   1/1     Running   0          87s
```

- expose the service
```
oc expose service/nginx           
route.route.openshift.io/nginx exposed
```
## create an edge route (https)

- prereq: generate tls `.crt` and `.key` from inside the provided `certs` directory (run `openssl-comands.sh`)
- create a secure route (edge)  
```
❯ oc create route edge nginx-https --service nginx --hostname nginx-https.apps-crc.testing --key <file>.key --cert <file>.crt
route.route.openshift.io/nginx-https created
```  
this route will be accessible with https://nginx-https.apps-crc.testing
test with the `curl -s -k https://<URL>` command


  
  
  [back to main](./README.md) 
