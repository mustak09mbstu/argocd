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

# Argo CD cli installation
Download With CurlDownload latest version

```bash
curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
chmod +x /usr/local/bin/argocd
```
Download selected version
Select desired TAG from https://github.com/argoproj/argo-cd/releases
VERSION=<TAG>
```bash
curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/$VERSION/argocd-linux-amd64
chmod +x /usr/local/bin/argocd
```
# Check version

```bash
argocd version
```
# Login Using The CLI
Retrieve initial password using the argocd CLI
```bash
argocd admin initial-password -n argocd
```
Using the username admin and the password from above, login to Argo CD's IP or hostname:
```bash
argocd login <ARGOCD_SERVER>

root@DESKTOP-OAM0BVD:~# argocd login 172.21.0.3:31736
WARNING: server certificate had error: tls: failed to verify certificate: x509: cannot validate certificate for 172.21.0.3 because it doesn't contain any IP SANs. Proceed insecurely (y/n)? y
Username: admin
Password:
'admin:login' logged in successfully
Context '172.21.0.3:31736' updated
```

# Change the password using the command
After successful login change the account password. After change logout and relogin to argocd server.
```bash
argocd account update-password
old-pass: R1w6jtpXCwqryf6-
new pass: argocd-admin
```

# Register A Cluster To Deploy Apps To (Optional)
This step registers a cluster's credentials to Argo CD, and is only necessary when deploying to an external cluster. When deploying internally (to the same cluster that Argo CD is running in), https://kubernetes.default.svc should be used as the application's K8s API server address.

First list all clusters contexts in your current kubeconfig:
```bash
kubectl config get-contexts -o name
```
Choose a context name from the list and supply it to argocd cluster add CONTEXTNAME. For example, for docker-desktop context, run:
```bash
argocd cluster add docker-desktop
```
The above command installs a ServiceAccount (argocd-manager), into the kube-system namespace of that kubectl context, and binds the service account to an admin-level ClusterRole. Argo CD uses this service account token to perform its management tasks (i.e. deploy/monitoring).

> [!NOTE]
The rules of the argocd-manager-role role can be modified such that it only has create, update, patch, delete privileges to a limited set of namespaces, groups, kinds. However get, list, watch privileges are required at the cluster-scope for Argo CD to function.

# Create An Application From A Git Repository
An example repository containing a guestbook application is available at ['https://github.com/argoproj/argocd-example-apps.git'] to demonstrate how Argo CD works.

Creating Apps Via CLI
First we need to set the current namespace to argocd running the following command:
```bash
kubectl config set-context --current --namespace=argocd
```

Create the example guestbook application with the following command:

```bash
argocd app create guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path guestbook --dest-server https://kubernetes.default.svc --dest-namespace default
```


# References
- https://argo-cd.readthedocs.io/en/stable/getting_started/
- https://github.com/argoproj/argo-helm/tree/main/charts/argo-cd
- https://gist.github.com/dmancloud/7a024aa0e47fd39bd0db6e80a4aae842


# Argocd CLI installation reference
- https://foxutech.medium.com/argo-cd-cli-installation-and-commands-65ab578ed75


# Demo Repo
- https://github.com/dmancloud/argocd-tutorial