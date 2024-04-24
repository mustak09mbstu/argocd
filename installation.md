# Install ArgoCD
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

# Change Service to NodePort
Edit the service can change the service type from ClusterIP to NodePort

```bash
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}' 
```
# Fetch Password
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
# Deploy Demo Application
You can use the below repository to deploy a demo nginx application

- https://github.com/mustak09mbstu/argocd

# Scale Replicaset
```bash
kubectl scale --replicas=3 deployment nginx -n default
```
# Clean Up
```bash
kubectl delete -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl delete namespace argocd
```



# References
- https://argo-cd.readthedocs.io/en/stable/getting_started/
- https://github.com/argoproj/argo-helm/tree/main/charts/argo-cd
- https://gist.github.com/dmancloud/7a024aa0e47fd39bd0db6e80a4aae842

# Demo Repo
- https://github.com/dmancloud/argocd-tutorial