# Horizontal scaling in K8s
Let's increase number of application PODs:
```bash
kubectl scale deployment -n echo echo-deployment --replicas=5
```
K8s LoadBalancer will distribute traffic incoming to the service between the application PODs
