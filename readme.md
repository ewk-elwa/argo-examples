

# Follow this video to be a ArgoCD Boss
https://youtu.be/JLrR9RV9AFA


# Installing latest/stable version of ArgoCD
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### Forward Ports
```
kubectl get services -n argocd
kubectl port-forward service/argocd-server -n argocd 8080:443 > /dev/null 2>&1 &
```

### Get Credentials
```
ARGO_PASS=$(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d)
echo $ARGO_PASS
```

# Install ArgoCD CLI / Login via CLI
```
brew install argocd
kubectl port-forward svc/argocd-server -n argocd 8080:443 > /dev/null 2>&1 &
argocd login localhost:8080 --username admin --password $ARGO_PASS --insecure

```

# Creating an Application using ArgoCD CLI:
```
argocd app create webapp-helm-prod \
--repo https://github.com/ewk-elwa/argo-examples.git \
--path helm-webapp --dest-server https://kubernetes.default.svc \
--dest-namespace dev

argocd app create webapp-kustom-prod \
--repo https://github.com/ewk-elwa/argo-examples.git \
--path kustom-webapp/overlays/prod --dest-server https://kubernetes.default.svc \
--dest-namespace prod
```

# Checking
```
# Check all pods running
kubectl get all -n dev

# Portforward the service
kubectl port-forward service/webapp-helm-prod -n dev 8888:80 > /dev/null 2>&1 &
curl localhost:8888
```

# Command Cheat sheet
```
kubectl get all -n <namespace>
kubectl port-forward service/<app> -n <namespace> <localport>:<containerport>  > /dev/null 2>&1 &
argocd app create #Create a new Argo CD application.
argocd app list #List all applications in Argo CD.
argocd app logs <appname> #Get the application’s log output.
argocd app get <appname> #Get information about an Argo CD application.
argocd app diff <appname> #Compare the application’s configuration to its source repository.
argocd app sync <appname> #Synchronize the application with its source repository.
argocd app history <appname> #Get information about an Argo CD application.
argocd app rollback <appname> #Rollback to a previous version
argocd app set <appname> #Set the application’s configuration.
argocd app delete <appname> #Delete an Argo CD application.
```





