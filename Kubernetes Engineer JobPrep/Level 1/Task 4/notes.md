### Task

- Create a pod named httpd-pod with a container named httpd-container. Use the httpd image with the latest tag (specify as httpd:latest). Set the following resource limits:

- Requests: Memory: 15Mi, CPU: 100m

- Limits: Memory: 20Mi, CPU: 100m

---

### Solution

To create a pod with a impertive command

```k run httpd-pod --image=httpd:latest --dry-run=client -o yaml > httpd.yaml```

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: httpd-pod
  name: httpd-pod
spec:
  containers:
  - image: httpd:latest
    name: httpd-pod
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

```

From the Kubernetes Documentation find the 
Assign Memory Resources to Containers and Pods

Resource Management for Pods and Containers: It really took me time to find out the exact documentation for hte manifest.

https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/?utm_source=chatgpt.com

```
Example:

spec:
  containers:
  - name: app
    image: images.my-company.example/app:v4
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"

```
Edit the manifest with the following values.

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: httpd-pod
  name: httpd-pod
spec:
  containers:
  - image: httpd:latest
    name: httpd-container
    resources:
      requests:
        memory: "15Mi"
        cpu: "100m"
      limits:
        memory: "20Mi"
        cpu: "100m"

```

``` k apply -f httpd.yaml``` to create the pod

```k describe pod httpd-pod``` check the properties.

Complete!




