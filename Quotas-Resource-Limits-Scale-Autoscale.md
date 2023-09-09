# Quotas - Resource Limits - Scale - Autoscale

- Deploy hello-nginx app to test with in project limits-scale
```
❯ oc new-project limits-scale        

❯ oc new-app --name hello --image quay.io/redhattraining/hello-world-nginx
```

- scale deployment to 2 pods


- Create an autoscale policy so that the deployment has a minimum 2 pods and a max of 4 pod and scales when cpu is over 75%  


- Create resource limits for deployment  
[] request cpu 100m  
[] limit cpu 200m  
[] request memory 20Mi  
[] limit cpu 100Mi  


  
- Create quota: `cpu 3`, `memory 1G`, `configmaps 2` in the curent project  

  
    [back to main](./README.md) 
  
