### Tasks

 1. Create a Service named myapp of type ClusterIP that exposes port 80 and maps to the target port 80.
```
apiVersion: v1
kind: Service
metadata:
  name: myapp
spec:
  ports:
    - name: myapp
      protocol: TCP
      port: 80
      targetPort: 80
```
<mark>what if there is no protocol. set?

```
controlplane ~ ➜  k describe  svc myapp 
Name:                     myapp
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 <none>
Type:                     ClusterIP
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       172.20.135.79
IPs:                      172.20.135.79
Port:                     myapp  80/TCP
TargetPort:               80/TCP
Endpoints:                <none>
Session Affinity:         None
Internal Traffic Policy:  Cluster
Events:                   <none>

```

---

2. Create a Deployment named myapp that creates 1 replica running the image nginx:1.23.4-alpine. Expose the container port 80.

``` controlplane ~ ➜  k create deployment myapp --image=nginx:1.23.4-alpine --port=80 --replicas=1 --dry-run=client -o yaml > 2.yaml ```

```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: myapp
  name: myapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: myapp
  strategy: {}
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - image: nginx:1.23.4-alpine
        name: nginx
        ports:
        - containerPort: 80
        resources: {}
status: {}

```

``` k apply -f 2.yaml```

``` k describe deployment myapp ```

```
controlplane ~ ➜  k describe deployments.apps myapp 
Name:                   myapp
Namespace:              default
CreationTimestamp:      Fri, 23 Jan 2026 19:33:23 +0000
Labels:                 app=myapp
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=myapp
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=myapp
  Containers:
   nginx:
    Image:         nginx:1.23.4-alpine
    Port:          80/TCP
    Host Port:     0/TCP
    Environment:   <none>
    Mounts:        <none>
  Volumes:         <none>
  Node-Selectors:  <none>
  Tolerations:     <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   myapp-7599c7c8c7 (1/1 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  9s    deployment-controller  Scaled up replica set myapp-7599c7c8c7 from 0 to 1

```

---

3. Scale the Deployment to 2 replicas.

``` k scale deployment myapp --replicas=2 ```

![](2026-01-24-01-06-59.png)

---

4. Create a temporary Pod using the image busybox and run a wget command against the IP of the service.

``` k run temp --image busybox --command wget 172.20.135.79 ```

``` controlplane ~ ➜  k describe pod temp
Name:             temp
Namespace:        default
Priority:         0
Service Account:  default
Node:             node02/192.168.121.251
Start Time:       Fri, 23 Jan 2026 19:43:30 +0000
Labels:           run=temp
Annotations:      cni.projectcalico.org/containerID: 56a9226882c1cd74c9ef4ddbd4250c84ab7c74df7a69347c61939aa40410e7b3
                  cni.projectcalico.org/podIP: 172.17.2.3/32
                  cni.projectcalico.org/podIPs: 172.17.2.3/32
Status:           Running
IP:               172.17.2.3
IPs:
  IP:  172.17.2.3
Containers:
  temp:
    Container ID:  containerd://8c35dc70881736952e3128b72a432006d35e83973617add12b8dbefb7292583f
    Image:         busybox
    Image ID:      docker.io/library/busybox@sha256:e226d6308690dbe282443c8c7e57365c96b5228f0fe7f40731b5d84d37a06839
    Port:          <none>
    Host Port:     <none>
    Command:
      wget
      172.20.135.79
    State:          Waiting
      Reason:       CrashLoopBackOff
    Last State:     Terminated
      Reason:       Error
      Exit Code:    1
      Started:      Fri, 23 Jan 2026 19:43:32 +0000
      Finished:     Fri, 23 Jan 2026 19:43:33 +0000
    Ready:          False
    Restart Count:  1
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-7sjm9 (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 True 
  Ready                       False 
  ContainersReady             False 
  PodScheduled                True 
Volumes:
  kube-api-access-7sjm9:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason     Age                From               Message
  ----     ------     ----               ----               -------
  Normal   Scheduled  12s                default-scheduler  Successfully assigned default/temp to node02
  Normal   Pulled     11s                kubelet            spec.containers{temp}: Successfully pulled image "busybox" in 150ms (150ms including waiting). Image size: 2222260 bytes.
  Normal   Pulling    10s (x2 over 11s)  kubelet            spec.containers{temp}: Pulling image "busybox"
  Normal   Created    10s (x2 over 11s)  kubelet            spec.containers{temp}: Container created
  Normal   Started    10s (x2 over 11s)  kubelet            spec.containers{temp}: Container started
  Normal   Pulled     10s                kubelet            spec.containers{temp}: Successfully pulled image "busybox" in 147ms (147ms including waiting). Image size: 2222260 bytes.
  Warning  BackOff    7s (x2 over 8s)    kubelet            spec.containers{temp}: Back-off restarting failed container temp in pod temp_default(bfcf8d7f-f85b-47e3-a3a3-f016b4009b4c)
  
  ```

  ---

  5. Run a wget command against the service outside the cluster.
  ---

  6. Change the service type so the Pods can be reached outside the cluster.

  7. Run a wget command against the service outside the cluster.

  8. Discuss: Can you expose the Pods as a service without a deployment?
  - I think the pod should exit to be expose to any service / deployment.

Discuss: Under what condition would you use the service types LoadBalancer, node port, clusterIP, and external?
- LoadBalancer - Cloud provider
- Node port - To communicate outside the cluster
- ClusterIP - To communicate within the cluster
- External - Dont know? need to research