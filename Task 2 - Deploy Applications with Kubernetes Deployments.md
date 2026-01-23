Type in the command

```
k create deployment nginx --image=nginx:latest
```
Next
```
k get deployments.apps nginx
```
Next
```
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   1/1     1            1           13s
```

Verify the image name is correct.
```
k describe deployments.apps nginx | grep -i image

Image:         nginx:latest
```

