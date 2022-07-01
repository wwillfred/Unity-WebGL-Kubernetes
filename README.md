# Deploying WebGL build of Unity game with Kubernetes

This simple Kubernetes manifest will deploy a container made from a Docker image that uses NGINX to serve the WebGL build of a **very** rough mockup of the game first-game.

***Note:*** The first section describes how to set up a resource group and cluster using Azure Kubernetes Service; if using Minikube this section can be skipped.

**For how to create a Docker image of a WebGL unity build,** see https://github.com/tomowatt/unity-docker-example 

### Provision AKS resources
```bash
$ az login
$ az group create --name GameRG --location "australiaeast"
$ az aks create --resource-group GameRG --name Game --node-count 1
$ az aks get-credentials --resource-group GameRG --name Game
$ kubectl config set-context Game
$ kubectl cluster-info
```

### Deploy the service
```bash
$ kubectl apply -f unity-build.yaml
$ kubectl get pods
$ kubectl get deployments
```

### View deployed page
```bash
$ kubectl get services
# wait until the "External IP" changes from <PENDING> to an actual IP address

$ open http://<External IP>
```
Then wait for your browser to open the page. You should now see a running WebGL build of your game!

### Remove AKS resources
```zsh
$ az aks stop --name Game --resource-group GameRG
$ az aks delete --name Game --resource-group GameRG
$ az group delete --name GameRG
```
