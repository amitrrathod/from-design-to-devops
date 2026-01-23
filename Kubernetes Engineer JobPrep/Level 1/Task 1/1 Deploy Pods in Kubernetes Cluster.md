<<<<<<< HEAD
### Tasks
The Nautilus DevOps team is diving into Kubernetes for application management. One team member has a task to create a pod according to the details below:

- Create a pod named pod-httpd using the httpd image with the latest tag. Ensure to specify the tag as httpd:latest.

- Set the app label to httpd_app, and name the container as httpd-container.

**Note: The kubectl utility on jump_host is configured to operate with the Kubernetes cluster.**

---

### Solution

Using the imperative command to build a yaml output.

```
k run pod-httpd --image=httpd:latest --labels=app=httpd_app --dry-run=client -o yaml > 1.yaml
```

Open the yaml and change the container name to the following httpd-container

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    app: httpd_app
  name: pod-httpd
spec:
  containers:
  - image: httpd:latest
    name: httpd-container
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

```

Run the command ```k apply -f 1.yaml```

Verify with the command ```k describe pod pod-httpd```

---

Learnings:
- checked the alias is set to k for kubectl utility.
- using the labels command to integrate from the command line.
- --dry-run=client to get a output in yaml format.
=======
### Solution

Using the imperative command to build a yaml output.

```
k run pod-httpd --image=httpd:latest --labels=app=httpd_app --dry-run=client -o yaml > 1.yaml
```

Open the yaml and change the container name to the following httpd-container

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    app: httpd_app
  name: pod-httpd
spec:
  containers:
  - image: httpd:latest
    name: httpd-container
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

```

Run the command ```k apply -f 1.yaml```

Verify with the command ```k describe pod pod-httpd```

---

Learnings:
- checked the alias is set to k for kubectl utility.
- using the labels command to integrate from the command line.
- --dry-run=client to get a output in yaml format.
>>>>>>> 16a544f402425c844dd6134678acf203df399f81
