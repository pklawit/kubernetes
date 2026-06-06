# K8s Deployments
Earlier we have created out application using explicit POD definitions.
Such POD will work on the cluster until somebody removes it or until it crushes.
For real HA application, we can use K8s deployment component to define our PODs.
In that case K8s will always watch our PODs and ensure, that it is always running. In case of failure - it will try to restart it or it will create new POD instance if the old one has disappeared.

## How it works with explicit POD definition:
1. Check the status of echo application POD:
```bash
kubectl get pod -n echo
```

2. Delete the old application POD:
```bash
kubectl delete pod -n echo ....
```

3. Check what happened on the cluster:
```bash
kubectl get pod -n echo
```
You may see, that the application POD has disappeared, and our echo service is no longer working

## Deployment example:
1. Apply the deployment.yaml file:
```bash
kubectl apply -f deployment.yaml
```

2. Wait until the POD is up and running.
```bash
kubectl get pod -n echo
```

3. Test the echo service from client POD


4. Delete the echo service POD
```bash
kubectl delete pod -n echo ......
```

5. Check what has happened to the POD and check if the echo application is still working


