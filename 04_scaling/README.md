# Horizontal scaling in K8s
Let's increase number of application PODs:
```bash
kubectl scale deployment -n echo echo-deployment --replicas=5
```
Check number of PODs in the namespace echo now:
```bash
kubectl get pod -n echo
```
You will see, that there are now 5 echo-deployment-... PODs. They all are running the same code and are ready to serve incoming TCP requests on port 8080.
K8s serivces 'echo-service' LoadBalancer will now distribute the traffic incoming to port 8080 between the application PODs.
Send couple messages from client-pod and check:

```bash
echo "Message 1" | nc -N echo-service 8080
```
```bash
echo "Message 2" | nc -N echo-service 8080
```
```bash
echo "Message 3" | nc -N echo-service 8080
```
```bash
echo "Message 4" | nc -N echo-service 8080
```
```bash
echo "Message 5" | nc -N echo-service 8080
```
```bash
echo "Message 6" | nc -N echo-service 8080
```

