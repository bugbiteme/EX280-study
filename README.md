# EX280-study
My study notes for Red Hat EX280 Certification (Openshift 4.12)

---

Here are some relevant tasks for the Red Hat EX280 Certification Exam.  
So that you know, this project only covers some topics in the exam. I have included exercises
found here for my study purposes.

## Categories Covered In EX280

- Manage OpenShift Container Platform   
    [Solution](./Manage-OpenShift-Container-Platform.md) 


- Deploy applications
  applications that can access secrets with key/val accessible via environment variables in pod  
  create app with deployment->add service->expose route (edge)
  create app via deployment
  add service to deployment  
  create route to service (edge?) 

- Manage storage for application configuration and data  create pv->pvc ReadOnlyOnce
 	[Challenge](./Manage-storage-for-application-configuration-and-data.md)  
    [Solution](./Manage-storage-for-application-configuration-and-data-SOLUTION.md)  

- Configure applications for reliability  (liveliness probe)
 	[Challenge](./Configure-applications-for-reliability.md)  
    [Solution](./Configure-applications-for-reliability-SOLUTION.md)

- Manage authentication and authorization  
 	[Challenge](./Manage-authentication-and-authorization.md)  
    [Solution](./Manage-authentication-and-authorization-SOLUTION.md)  

- Configure network security  (edge with cert/key and maybe CA cert?)
 	[Challenge](./Configure-service-account-edge-passthoguh-routes.md)  
    [Solution](./Configure-service-accountedge-passthoguh-routes-SOLUTION.md) 

- Enable developer self-service
  create a default project template with a limit range (max-min-default-request)
  make that the default setting when someone creates a new project   
    [Challenge](./Enable-developer-self-service.md)  
    [Solution](./Enable-developer-self-service-SOLUTION.md)  
  
- Manage OpenShift operators

- Configure application security  (source pod selector with deployment label and added label [name=value]) port access as well
 	[Challenge](./Configure-application-security.md)  
    [Solution](./Configure-application-security-SOLUTION.md) 

- Quotas - Resource Limits - Scale - Autoscale  
 	[Challenge](./Quotas-Resource-Limits-Scale-Autoscale.md)  
    [Solution](./Quotas-Resource-Limits-Scale-Autoscale-SOLUTION.md)   

- cron jobs in OpenShift  (at a certain time on x day of month) add service account to job?
 	[Challenge](./cron-jobs-in-OpenShift.md)  
    [Solution](./cron-jobs-in-OpenShift-SOLUTION.md)   

---

## My Lab Environment
My lab environment consists of a mix of Red Hat Code Ready Containers (CRC) and the lab environment provided by DO280R.  

My CRC environment is on OpenShift 4.13, and I found that the procedure for removing the kubeadmin account differs from 4.12 in the DO280 lab environment.  

Although all of these can be run on your CRC environment, the DO280 lab environment has some of the images I use to deploy, as well as NFS, set up ahead of time for testing Persistent Volume Claims (PVC) and some of the certs created for network security exercises. 

---
Topics not covered here that you should still study for  
  
- Install a Helm chart and deploy an app with it (use helm CLI to install chart and web consol to deploy)
- associate key/value secret for application
- LimitRanges
- install openshift operators (via web console)



