# Deployment and Rolling update
Parameter present in YAML:
- apiVersion: apps/v1
- kind: Deployement
- metadata: name of deployment and labels for deployment
- specs: here main configuration related to replicaset and container are present.
    - strategy: here we define what is strategy for deploying new changes. there are two types of strategy for this one is Rolling update and recreate.
        - Rolling Update: here we define two parameter maxSurge and maxUnavailable paramerter. maxSurge define how many pods are deploy simultenlously and maxUnavailable define how many pods should be terminated. both are present in number or percentage depends upon use case.
    - revisionHistorylimit: By default is 10, this will maintain history of replicaset for rollout. suipose we set to 0 then there is no chance to rollout as history is not maintain.

    command to see all replicaset (active and inactive )    
        ``` kubectl get replicaset -l  ```
    - replicaset : how many number of pods are created. 
    - selector: selector define which pods are controlled by deploymnets. 
    - template: template for container running in pod
        - metadata : here we define name and labels of pods. label define here is used by selector mentioned above for managing pods.
        - specs : specification of pods like image, name of pod and port

# Undo Rolling Update
This will useful when we have to rollback our changes to previous version or desire version.

    kubectl rollout undo deployment/name_of_deployment

go to specific version

     kubectl rollout undo deployment/name_of_deployment --to-revision=2

# Pausing and Resume deployment
When you update a Deployment, or plan to, you can pause rollouts for that Deployment before you trigger one or more updates. When you're ready to apply those changes, you resume rollouts for the Deployment. This approach allows you to apply multiple fixes in between pausing and resuming without triggering unnecessary rollouts.

commands:

    kubectl rollout pause deployment/name_of_deployment

    kubectl rollout resume deployment/name_of_deployment

# Set and Edit
with the help of set and edit we can edit deployment configuration imperative way.

commands:

    kubectl set image deployment/nginx-deployment nginx=nginx:1.16.1

    kubectl edit deployment deployment/nginx-deployment

# Scale
with the help of scale command we can change replicaset without changing deployment file and this called imperative change.

    kubectl scale deployment/ nginx-deployment --replicas=10


