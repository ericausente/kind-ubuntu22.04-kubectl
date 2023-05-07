# kind-ubuntu22.04-kubectl


First, create a Kind cluster by running the following command:

kind create cluster

Deploy a sample application in the Kind cluster, for example, a simple "Hello World" application using a Kubernetes Deployment and Service YAML file like this:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world
spec:
  replicas: 3
  selector:
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
      - name: hello-world
        image: gcr.io/google-samples/hello-app:2.0
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: hello-world
spec:
  type: NodePort
  selector:
    app: hello-world
  ports:
  - port: 8080
    targetPort: 8080
    nodePort: 30000
```

Save this file as hello-world.yaml and then deploy it using the following command:

kubectl apply -f hello-world.yaml

Verify that the service is up and running by checking its status using the following command:

kubectl get services

You should see output similar to the following:
```
NAME          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
hello-world   NodePort    10.107.64.202   <none>        8080:30000/TCP   10m
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP          7d18h
```

To access the service from your local machine, you need to know the IP address of your Docker interface. You can find this by running the following command:

ip addr show docker0

This will show you output similar to the following:

```
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether 02:42:2e:0c:91:1a brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
```

In this example, the IP address of the Docker interface is 172.17.0.1.

Now that you know the IP address of your Docker interface, you can access the service from your local machine by opening a web browser and navigating to http://172.17.0.1:30000. This should display the "Hello World" application running in the Kind cluster.

Alternatively, you can test the service using the curl command from your local machine by running the following command:

```
    curl http://172.17.0.1:30000
```
    This should output the response from the "Hello World" application, which should be "Hello, World!".

That's it! You have successfully exposed a service as a NodePort in Kind and accessed it from your local machine.
