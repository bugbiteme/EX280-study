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
- We will be using nfs-storage

- It is easiet to do this with the GUI, otherwise you need to create a yaml file from scratch
Storage->PersistentVolumeClaims
![screenshot](img/image5.png)
  
Create PersistenVolumeClaim
![screenshot](img/image6.png)

Fill out form with the following info
```
PersistentVolumeClaim name: storage-test-pvc
Access mode: Single user (RWO)
Size: 1Gi
Volume mode: Filesystem
```

yaml looks like this
```
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: storage-test-pvc
  namespace: storage-test
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: nfs-storage
  volumeMode: Filesystem
```
- add PVC to storage-test-app deployment
(Using GUI)  
  
From the deployment:  
`Actions->Add storage`  

```
(0)Use existing claim: storage-test-pvc

Mount path: /mnt/storage-test

[Save]
``` 

App will redeploy 

- log into pod and test storage

```
$ oc rsh storage-test-app-54bdc95c84-tq4zx /bin/bash
bash-4.4$ ls /mnt
storage-test

$ echo "hello">/mnt/storage-test/hello.txt

$ cat /mnt/storage-test/hello.txt 
hello
```
- delete pod, and log into the new pod to make sure the hello.txt file still exists

```
$ oc delete pod storage-test-app-54bdc95c84-tq4zx 
pod "storage-test-app-54bdc95c84-tq4zx" deleted

$ oc get pods
NAME                                READY   STATUS    RESTARTS   AGE
storage-test-app-54bdc95c84-sllnm   1/1     Running   0          12s

$ oc rsh storage-test-app-54bdc95c84-sllnm cat /mnt/storage-test/hello.txt
hello
```

...
  
  
  [back to main](./README.md) 