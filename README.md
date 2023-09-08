# EX280-study
My study notes for Red Hat EX280 Certification (Openshift 4.12)

---

Here are some relevant tasks for the Red Hat EX280 Certification Exam.  
So that you know, this project only covers some topics in the exam. I have included exercises
found here for my study purposes.

## Categories Covered In EX280

- Manage OpenShift Container Platform 


- Deploy applications 

- Manage storage for application configuration and data  
 	[Challenge](./Manage-storage-for-application-configuration-and-data.md)  
    [Solution](./Manage-storage-for-application-configuration-and-data-SOLUTION.md)  

- Configure applications for reliability 

- Manage authentication and authorization  
 	[Challenge](./Manage-authentication-and-authorization.md)  
    [Solution](./Manage-authentication-and-authorization-SOLUTION.md)  

- Configure network security

- Enable developer self-service

- Manage OpenShift operators

- Configure application security
---

## My Lab Environment
My lab environment consists of a mix of Red Hat Code Ready Containers (CRC) and the lab environment provided by DO280R.  

My CRC environment is on OpenShift 4.13, and I found that the procedure for removing the kubeadmin account differs from 4.12 in the DO280 lab environment.  

Although all of these can be run on your CRC environment, the DO280 lab environment has some of the images I use to deploy, as well as NFS, set up ahead of time for testing Persistent Volume Claims (PVC) and some of the certs created for network security exercises. I may include certs/keys in this repo.  


