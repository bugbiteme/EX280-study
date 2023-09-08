# Manage storage for application configuration and data

- Create a project called `storage-test`

- Create new app called `storage-test-app`

`$ oc new-app --name storage-test-app --image quay.io/redhattraining/hello-world-nginx`  
  
- Check available storage classes in cluster

```
$ oc get storageclasses
NAME                    PROVISIONER                                   RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
lvms-vg1                topolvm.io                                    Delete          WaitForFirstConsumer   ...
nfs-storage (default)   k8s-sigs.io/nfs-subdir-external-provisioner   Delete          Immediate   ...           
```
- Add NFS volume to running app


- delete pod, and log into the new pod to make sure the data still exists  
  
  [back to main](./README.md) 

