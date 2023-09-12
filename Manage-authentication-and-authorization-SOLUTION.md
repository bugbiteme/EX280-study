# SOLUTION
# Manage authentication and authorization (Can be done in CRC)

## Tasks
### Configure the cluster to use and HTPasswd identity provider  
- create user accounts for: `admin`, `leader`, `developer` and `qa-engineer`

1. login as kubeadmin

`oc login kubeadmin`


2. Use htpasswd to create account logins

```
❯ htpasswd -c -B -b users.htpasswd admin redhat   
Adding password for user admin


❯ htpasswd -B -b users.htpasswd leader redhat
Adding password for user leader


❯ htpasswd -B -b users.htpasswd developer redhat
Adding password for user developer


❯ htpasswd -B -b users.htpasswd qa-engineer redhat
Adding password for user qa-engineer
```
- Create a secret called `cluster-users-secret` using the htpasswd credentails

```
❯ oc create secret generic cluster-users-secret --from-file htpasswd=users.htpasswd -n openshift-config
secret/cluster-users-secret created
```

- create an identiy provider called `cluster-users` that reads the `cluster-users-secret` secret

Note I like to use the web-console (as `kubeadmin`)for this step, since via the command line requires creating `yaml` file, and I can't remember the format, so GUI it is!

Administration->Cluster Settings->Configuration->Oauth

Identity pvoviders->Add->HTPasswd

use `cluster-users` as the name, and just type something random in the bottom window
Add

Edit the yaml in the GUI and edit so the blog looks like this

```
...
- htpasswd:
        fileData:
          name: cluster-users-secret
      mappingMethod: claim
      name: cluster-users
      type: HTPasswd
...
```
and click Save

watch `oc get pods -n openshift-authentication` to make sure the pod relaunches
```
 Every 2.0s: oc get pods -n openshift-authentication                                                                                                                    server-name: Thu Sep  7 14:07:05 2023

NAME                               READY   STATUS        RESTARTS   AGE
oauth-openshift-7874464f79-6kpvq   1/1     Terminating   0          37m
oauth-openshift-86dd74b64-tzbs6    0/1     Pending       0          23s
.
.
.
NAME                              READY   STATUS    RESTARTS   AGE
oauth-openshift-86dd74b64-tzbs6   1/1     Running   0          61s
```
The above can take a few minutes to relaunch

login to each account to make sure authentication works

```
oc login -u admin -p redhat
Login successful.

You don't have any projects. You can try to create a new project, by running

    oc new-project <projectname>
```


### Account permissions

log back in as kubeadmin

- `admin` should be able to modify the cluster

```
oc adm policy add-cluster-role-to-user cluster-admin admin
clusterrole.rbac.authorization.k8s.io/cluster-admin added: "admin"
```

log back in as admin, and you should be able to perform the following

- `leader` should be able to create projects

```
oc adm policy add-cluster-role-to-user self-provisioner leader
clusterrole.rbac.authorization.k8s.io/self-provisioner added: "leader"
```

- `developer` and `qa-engineer` should not be able to modify the cluster

```
❯ oc adm policy add-role-to-user view developer -n openshift-config
clusterrole.rbac.authorization.k8s.io/view added: "developer"


❯ oc adm policy add-role-to-user view qa-enginer -n openshift-config
clusterrole.rbac.authorization.k8s.io/view added: "qa-engineer"
```

- No other user should be able to create a project

```
oc delete rolebinding self-provisioner 
rolebinding.rbac.authorization.k8s.io "self-provisioner" deleted
```

### Default account cleanup
- Remove the `kubeadmin` account 

(for 4.12)
`oc delete secrets -n kube-system kubeadmin`  
  
(for 4.13)
```
❯ oc delete user kubeadmin                  
user.user.openshift.io "kubeadmin" deleted

❯ oc delete clusterrolebinding kubeadmin
clusterrolebinding.rbac.authorization.k8s.io "kubeadmin" deleted
```

### Project creation
- Create three projects: `front-end`, `back-end` and `app-db`

`oc new-project ...`

- `leader` user will be the admin of the projects

```
❯ oc adm policy add-role-to-user admin leader -n front-end  
clusterrole.rbac.authorization.k8s.io/admin added: "leader"

❯ oc adm policy add-role-to-user admin leader -n back-end 
clusterrole.rbac.authorization.k8s.io/admin added: "leader"

❯ oc adm policy add-role-to-user admin leader -n app-db  
clusterrole.rbac.authorization.k8s.io/admin added: "leader"
```

- `qa-engineer` user will have `view` access to the `app-db` project
```
❯ oc adm policy add-role-to-user view qa-engineer -n app-db
clusterrole.rbac.authorization.k8s.io/view added: "qa-engineer"
```
### Group management
- As `admin` create three user groups: `leaders`, `developers` and `qa`

```
❯ oc adm groups new leaders                               
group.user.openshift.io/leaders created

❯ oc adm groups new developers
group.user.openshift.io/developers created

❯ oc adm groups new qa        
group.user.openshift.io/qa created
```

- Add the `leader` user to the `leaders` group
`❯ oc adm groups add-users leaders leader`  

- Add the `developer` user to the `developers` group
`oc adm groups add-users developers developer`  

- Add the `qa-engineer` to the`qa` group
`oc adm groups add-users qa qa-engineer`  

- The `leaders` group should have edit access to `back-end` and `app-db`  

```
❯ oc adm policy add-role-to-group edit leaders -n back-end 
clusterrole.rbac.authorization.k8s.io/edit added: "leaders"


❯ oc adm policy add-role-to-group edit leaders -n app-db  
clusterrole.rbac.authorization.k8s.io/edit added: "leaders"
```

- The `qa` group should be able to `view` the `front-end` project but not `edit` it

`oc adm policy add-role-to-group view qa -n front-end`
  
  
  [back to main](./README.md) 
