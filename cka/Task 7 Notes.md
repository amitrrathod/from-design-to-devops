There are two types of ways you can create pod Imperative
Imperative command means simple typing the command create pod instructing the API server or Kubectl utility to create the object, get the details etc.
Declarative - To create a config file JSON / YAML, YAML is more human readable and easy to understand.
This are the two way to interact with the cluster via kubectl utility.
Before moving further I always like to set the alias for the kubectl utility which save a lot of time for me. alias k='kubectl'

Create an nginx pod through kubectl imperative way
```bash kubectl run nginx-pod --image=nginx:latest``` (if you image has tags you can put as :latest :1.24 version)
- Check the ready status: READY 1/1 
- If there are multiple/helper container you see 2/2

Basics of Yaml

There are 4 Top level properties mandatory fields for the yaml file
apiVersion / kind / metadata / spec

```yaml
apiVersion: v1   # string
kind: Pod        # string
metadata:        # Dictionary
  name: nginx-pod
  labels:            # you can add any key=value pairs under labels.
    app: myapp
spec:            
  containers:    # Array/Lists
    - name: nginx
      image: nginx:latest
```

Name as you wish I would name this task7-pod.yaml
And now lets run the command ```bash k crete -f task7-pod.yaml``` similarly you can use apply instead of create it helps to create/update the object.

To delete the pod
```bash k delete pod task7-pod```

Lets intentionally edit the image name to lets say redis123 and create the pod using the command or edit the running pod.

We will encounter the error with the status of ImagePullBackOff that means its face to pull the image from the dockerhub repositories. 
Checking the pod details will give more inputs under the events section.
To get the details of the pod ```k describe pod pod-name```

Directly update the object on the cluster 
```k edit pod pod-name``` # here you can make the changes on the running pod itself. It does not give all the fields to edit because you are making the changes on a live object.

Link to understand basics of yaml: https://www.youtube.com/watch?v=_f9ql2Y5Xcc&list=PLl4APkPHzsUUOkOv3i62UidrLmSB8DcGC&index=9
