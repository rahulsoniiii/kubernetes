## Provisioning EBS Volumes
- Create a storageclass.yaml file and apply the storageclass.yaml file using the following command:
```
kubectl apply -f storageClassLocal.yaml
```

- Create a pvc.yaml file and apply the pvc.yaml file using the following command:
```
kubectl apply -f pvc.yaml
```

- Create a deployment.yaml file and apply the deployment.yaml file using the following command:
```
kubectl apply -f deployment.yml
```

- Create a svc.yaml file and apply the svc.yaml file using the following command:
```
kubectl apply -f svc.yml
```

- Port Forward the service using the following command:
```
kubectl port-forward svc/nginx-svc 80:80
```

- Accessing the nginx webserver:
```
http://127.0.0.1:80
```