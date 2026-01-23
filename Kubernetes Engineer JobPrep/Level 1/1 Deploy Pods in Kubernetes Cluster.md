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
