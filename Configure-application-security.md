# Configure application security

This is an excercise where you configure security between pods.  

## Scenario A: Pods within a namespace/project can only talk to one another 
## Scenario B: Pods between namespaces/projects can talk to one another  

- create a project called `network-policy` 
- create two deployments/apps (`hello`, `test`) with the same image
```
oc new-app --name hello --image quay.io/redhattraining/hello-world-nginx  

oc new-app --name test --image quay.io/redhattraining/hello-world-nginx

```
- expose the hello service
```
❯ oc expose service/hello   
route.route.openshift.io/hello exposed

❯ curl hello-network-policy.apps-crc.testing                                    
<html>
  <body>
    <h1>Hello, world from nginx!</h1>
  </body>
</html>
```

- get wide output from pods,services,routes to get internal pod IP address and cluster IP addresses


- Access the `hello` pod from the `test` pod using `hello`'s pod and cluster IP

  
- create a second project called `different-project` and deploy the same image with the app name `sample-app`

```
❯ oc new-project different-project                                       
Now using project "different-project" on server "https://api.crc.testing:6443".

❯ oc new-app --name sample-app --image quay.io/redhattraining/hello-world-nginx
```
- access the `hello` pod from the `sample-app` pod via pod and cluster IP


- switch back to `network-policy` project and create a "deny-all" network policy

- test to make sure the pods cant access one another in the project (`test` trying to connect to `hello` pod and network IP)

- validate pod from `different-project` can no longer access `network-policy/hello`


- switch back to `network-policy` project and create a policy that will allow `different-project/sample-app` to access pods


- create lable `network=different-project` for project `different-project`

- access the `hello` pod from the `sample-app` pod via pod and cluster IP 
  
  
  [back to main](./README.md) 