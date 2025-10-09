# Movies Finder Deployment



## Getting started
### Create a helm chart
```bash
helm create web-app
```
### Install the helm chart
```bash
helm install web1 web-app/
```
### Update the helm chart
```bash
helm upgrade web1 web-app/
```
### Uninstall the helm chart
```bash
helm uninstall web1
```

## For secret
### Create secret
```bash
# encode
echo -n "password" | bashe4 -i -
# decode
echo cGFzc3dvcmQ= | base64 --decode
```
```yaml
# secret.yaml
apiVersion: v1
data:
  API_KEY: cGFzc3dvcmQ=
kind: Secret
metadata:
  name: myapp-secrets
```
or
```bash
kubectl create secret generic myapp-secrets   --from-literal=API_KEY="super-secret-key"   --dry-run=client -o yaml > secret.yaml
```
### To seal the secret
```bash
kubeseal --scope cluster-wide --controller-name=sealed-secrets --controller-namespace=kube-system --format yaml < secret.yaml > sealed-secret.yaml
```
### Move seal secret into templates folder to let argocd to apply it automatically