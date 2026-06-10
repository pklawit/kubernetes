# Demo - Echo responder application and database for conversation history

## Clone this repository to the VM:
```bash
git clone https://github.com/pklawit/kubernetes.git
```
## Go to the 02_echo-application folder:
```bash
cd kubernetes/02_echo-application/
```

## Deployment
Install all the components with command:
```bash
kubectl apply -f install_all.yaml
```

Wait a minute untill all PODs will have status 'Running':
```bash
kubectl get pods -n echo -w
```

## Execute the excercise

1. Watch 'database' logs
In new terminal window monitor the content of database log file:
```bash
kubectl exec -n echo db-pod -- tail -f /tmp/chat.log
```
For now it will be empty

2. Send something from client POD to the Echo responder POD
In another terminal window go inside the client POD:
```bash
kubectl exec -it -n echo client-pod -- /bin/sh
```
You are now inside the client POD. Let's sent some text message over TCP with netcat to the echo-service:
```bash
nc -N echo-service 8080 `echo Hi, this is message from client POD!`
```

Expected result:
You should immediately see response from the Echo service PDO:

<pre>Received: 'Hi, this is message from client POD! [Pod IP:10.42.0.30] [Time:2026-06-10 04:47:21]</pre>

3. Verify if the conversation is stored on the database POD
Go back to the terminal window where the DB log is printed out.
You should see the conversation sent from client POD stored there:
<pre>Hi, this is message from client POD!</pre>

