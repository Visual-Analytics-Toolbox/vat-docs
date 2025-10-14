# Developer Setup with local Kubernetes Cluster
You can install a local kubernetes cluster with kind:

```bash
sudo apt install golang-go
go install sigs.k8s.io/kind@v0.30.0
```

Add `go/bin` folder to your path. For example `export PATH="$HOME/go/bin:$PATH"`


Create the kubernetes cluster like this
```bash
kind create cluster --config kind-config.yaml
```
A tested config can be found at https://github.com/Visual-Analytics-Toolbox/Core-Infrastructure/blob/main/kind/kind-config.yaml

The script to deploy all helm charts needed for local deployment are in the Deployments repo: https://github.com/Visual-Analytics-Toolbox/VAT-Deployments

By default you can't pull our images from github. You need to be added to the Visual Analytics Org first and then create a Personal Access Token and add this to the cluster.
```bash
kubectl create secret docker-registry ghcr-secret \
  --docker-server=ghcr.io \
  --docker-username=YOUR_GITHUB_USERNAME \
  --docker-password=ghp_YOUR_TOKEN_HERE
```
and then add this to the default serviceaccount:
```bash
kubectl patch serviceaccount default \
  -p '{"imagePullSecrets": [{"name": "github-access"}]}' \
  --namespace=default
```