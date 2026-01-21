Task Details:
Document what you learned from the video in a blog, embed the video, and include all the references in the blog.

**Task 1:**
Create a pod using the imperative command and use nginx as the image?

**Solutions:**
```kubectl run nginx --image nginx```
This will create a pod with the name of nginx and uses the image nginx from the docker repo.

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Task 2:**

**Solutions:**
```k run nginx-new --image nginx --dry-run=client -o yaml```  # this command output the yaml structure. If you want to save the file the use the > @filename

```apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx-new
  name: nginx-new
spec:
  containers:
  - image: nginx
    name: nginx-new
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Task 3:**
Apply the below YAML and fix the errors, including all the commands that you run during the troubleshooting and the error message

```
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: test
  name: redis
spec:
  containers:
  - image: rediss
    name: redis
```

**Solutions:**
First create a file on the terminal redis.yaml and paste the yaml format.

```
apiVersion: v1
kind: Pod
metadata:
  name: redis
  labels:
    app: test
spec:
  containers:
  - image: redis
    name: redis
```
