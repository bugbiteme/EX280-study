# Enable developer self-service

1. Create a project `template-builder`
2. The workloads in the project cannot request a total of more than 1 GiB of RAM, and they cannot use more than 2 GiB of RAM.  
- create a quota for this:  
  
```
oc create quota memory --hard=requests.memory=1Gi,limits.memory=2Gi -n template-builder
```
3. Each workload in the project has the following properties:
  
```
Default memory request of 256 MiB  
  
Default memory limit of 512 MiB  
  
Minimum memory request of 128 MiB  
  
Maximum memory usage of 1 GiB  
```
- Create limit range `memory` in using the web console
```
apiVersion: v1
kind: LimitRange
metadata:
  name: memory
  namespace: template-test
spec:
  limits:
  - defaultRequest:
      memory: 256Mi
    default:
      memory: 512Mi
    max:
      memory: 1Gi
    min:
      memory: 128Mi
    type: Container
```
- test by deploying an app and scaling past quota
```
oc create deployment -n template-test test-limits --image quay.io/redhattraining/hello-world-nginx

oc scale --replicas=10 deployment/test-limits

oc get all
```
4. Create a project temlate definition with the same properties
- create bootstrap template
```
$ oc adm create-bootstrap-project-template -o yaml >template.yaml
```
- output the created quota and limitranges names `memory` and redirect to `>> template.yaml`
- move quota and limit above `parameters` and replace all instances of `template-builder` with `${PROJECT_NAME}`
- be sure to indent these lines 2 spaces
- be sure `apiVersion` object tags have the `-` in front and are in alignment with the included `- apiVersion: rbac...` object
- remove `creationTimeStamp`and other unnecessary tags/values

```
creationTimestamp

resourceVersion

uid

status
```
(see: `template-example.yaml` for example results)  

5. Create and configure the project template
- create in `openshift-config` namespace

```
$ oc create -f template.yaml -n openshift-config
template.template.openshift.io/project-request created
```
- use the `oc edit` command to change the cluster project configuration

```
oc edit projects.config.openshift.io cluster
```
(note: name will tab-complete)

- add template information under `spec:` tag
```
spec:
  projectRequestTemplate:
    name: project-request
```
- once saved, watch the pods in openshift-apiserver for an update (can take over a minute)

```
watch oc get pod -n openshift-apiserver
```
6. Create a project to make sure it works as intended and check `resourcequotas` and `limitranges` to see if present