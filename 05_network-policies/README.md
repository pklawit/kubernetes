# NetworkPolicies in K8s
By default K8s allows network traffic between all PODs in same namespace.
To make it more secure - we can deploy Network Policies.
First we will deploy NP blocking all incoming traffic to database POD.
Then (with another NP) we will enable only traffic between the Echo application POD and the database POD.

## Checking initial status (all network traffic allowed)
Let's check if the Echo application POD can still talk to the DB POD.
Enter the client POD and send sample message to Echo service:
```bash
kubectl exec -it -n echo client-pod -- /bin/sh
```
# and then inside the POD:
```ash
echo "Message before first NetworkPolicy" | nc -N echo-service 8080
```
Then check the log of database POD - the message should be visible there

## Applying 'deny-all' NetworkPolicy
Let's apply the NetworkPolicy, which will block all traffic incoming to database POD:
```bash
kubectl apply -f networkpolicy1.yaml
```

## Test connection between Echo POD and database POD with 'deny-all' NetworkPolicy
Go back to the client POD and send next message to Echo application POD:
```bash
kubectl exec -it -n echo client-pod -- /bin/sh
# and then inside the POD:
echo "Message with NP blocking traffic to database POD" | nc -N echo-service 8080
```
What will happen?
Command in the client POD will hang for a while, but finally it will get response from the echo-application POD.
Client sends the message to Echo service. This connection still works.
The Echo POD receives the message and tries to forward it to database POD with the code:
<pre>echo "Log: ..." | nc db-service 5432</pre>
At that moment the NetworkPolicy catches the packets sent to database POD and drops them.
The Echo application POD waits for TCP connection confirmation with database POD, and the client POD waits for the response from Echo application POD. The whole chain gets blocked.
If we check the logs on database POD - the new message will not appear there.


## Repair connection - new NetworkPolicy
Let's apply new NetworkPolicy, that will allow incoming traffic to database POD, but only from the Echo application POD:
```bash
kubectl apply -f networkpolicy2.yaml
```
Once this NP has been applied, the traffic between Echo application POD and database POD has been restored, but if anybody would try to send something to database POD from any other source than the Echo application POD - it would be blocked.

