- Create a namespace named dev and deploy a POD within it.
-  Name the pod dev-nginx-pod and use the nginx image with the latest tag.
-  Ensure to specify the tag as nginx:latest.

### Solution

As I am getting overwhelm by the content and command I am now approach the tasks with the --help pages for command excuition.

- So first mistake
Typing directly thinking it might work first - ```k create -n dev```
A hugh pile of content appeared that mean you dont know the command or syntax.

Later its becomes simple lets first do a ```k create namespace --help```
Under the Usage:
  kubectl create namespace NAME [--dry-run=server|client|none] [options]

  Now its much simplier to know how to type the correct command.

  So, ```k create namespace dev``` to verify ```k get ns```

```
NAME                 STATUS   AGE
default              Active   20m
dev                  Active   4s
kube-node-lease      Active   20m
kube-public          Active   20m
kube-system          Active   20m
local-path-storage   Active   20m

```

2 step is to create a pod with in the namespace.

```k run dev-nginx-pod --image=nginx:latest -n=dev```
pod/dev-nginx-pod created

Verify
```k get pods -n=dev```
dev-nginx-pod   1/1     Running   0          11s

-----------

Describe the pod to verify the pod values.
```k describe pod dev-nginx-pod -n=dev```

Checked All Done