glagent-eMjEPTu-CsAqafz4fTybiyszPnAh4FthGyH71nL21hyKUZry7g


Ninad


gldt-E1y5yYUDdmns6t9zNvz6



registry.gitlab.com/ninjacloud/docker-k8s:c9c88230

```
kubectl create secret docker-registry gitlab-regcred \
  --docker-server=registry.gitlab.com \
  --docker-username=ninad \
  --docker-password=gldt-E1y5yYUDdmns6t9zNvz6 
```

sudo gitlab-runner register


```
apiVersion: v1
kind: Pod
metadata:
  name: gitlab-pod
spec:
  containers:
    - name: gitlab-container
      image: registry.gitlab.com/ninjacloud/docker-k8s:c9c88230
      ports:
        - containerPort: 80
  imagePullSecrets:
    - name: gitlab-regcred
```



