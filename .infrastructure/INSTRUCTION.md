## 1. How to apply all manifests

To apply all manifests, you can use the `kubectl apply` command. Better to apply them in the following order:

```
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/busybox.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
```

## 2. How to test ToDo application using the `port-forward` command

To test the ToDo application using the `port-forward` command, you can run the following command in your terminal:

```
kubectl port-forward pod/todoapp-pod 8000:8000 -n todoapp
```

And then you can access the application in your browser at `http://localhost:8000/`.
Or you can test the API with curl:

```
curl http://localhost:8000/api/health/readiness/
curl http://localhost:8000/api/health/liveness/
```

## 3. How to test the application using the `busyboxplus:curl` container

To test the application using the `busyboxplus:curl` container, you can execute the following command in your terminal:

```
TODOAPP_IP=$(kubectl get pod todoapp-pod \
  -n todoapp \
  -o jsonpath='{.status.podIP}')

kubectl exec manifest-busybox -n todoapp -- \
  curl "http://${TODOAPP_IP}:8000/api/health/readiness/"
  
kubectl exec manifest-busybox -n todoapp -- \ 
  curl "http://${TODOAPP_IP}:8000/api/health/liveness/"
```