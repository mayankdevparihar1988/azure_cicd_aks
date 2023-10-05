Start Docker Images
run  = docker-compose -f docker-compose.yml -f docker-compose.override.yml up -d
stop = docker-compose -f docker-compose.yml -f docker-compose.override.yml down
--
See images
docker images

See running containers
docker ps
--
See application locally
TEST
http://localhost:8000/swagger/index.html
http://localhost:8001/
--
Stop Running Containers
stop = docker-compose -f docker-compose.yml -f docker-compose.override.yml down
-- --
Install the Azure CLI
	https://docs.microsoft.com/en-us/cli/azure/install-azure-cli
	https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?tabs=azure-cli
--
az version

{
  "azure-cli": "2.16.0",
  "azure-cli-core": "2.16.0",
  "azure-cli-telemetry": "1.0.6",
  "extensions": {}
}
--
az login
--
Create a resource group
az group create --name myResourceGroup --location westeurope
--
Create an Azure Container Registry
az acr create --resource-group myResourceGroup --name shoppingacrmayankmayank --sku Basic
--
Enable Admin Account for ACR Pull
az acr update -n shoppingacrmayankmayank --admin-enabled true
--
Log in to the container registry
az acr login --name shoppingacrmayankmayank
Login Succeeded
--
Tag a container image

get the login server address
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
shoppingacrmayankmayank.azurecr.io
--
Tag your images

docker tag shoppingapi:latest shoppingacrmayankmayank.azurecr.io/shoppingapi
docker tag shoppingclient:latest shoppingacrmayankmayank.azurecr.io/shoppingclient

Check
docker images
--
Push images to registry

docker push shoppingacrmayankmayank.azurecr.io/shoppingapi
docker push shoppingacrmayankmayank.azurecr.io/shoppingclient
--
List images in registry
az acr repository list --name shoppingacrmayankmayank --output table

Result
shoppingapi
shoppingclient
--
See tags
az acr repository show-tags --name shoppingacrmayankmayank --repository shoppingclient --output table

Result
v1
--
Create AKS cluster with attaching ACR
az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 1 --generate-ssh-keys --attach-acr shoppingacrmayankmayank

--
Install the Kubernetes CLI
az aks install-cli
--
Connect to cluster using kubectl
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster


--

Creating kubernetes-secrect 


Kubectl create secret docker-registry acr-secret —docker-server=shoppingacrmayank.azurecr.io
—docker-username=shoppingacrmayankmayank
—docker-password= yzncWrjo6gCQbRznECWPnXBKV4Bmq+RwNy+ovkD6aZ+ACRD3sttH
—docker-email=mayankazure1988@gmail.com

--

To verify;
kubectl get nodes
kubectl get all
--
Check Kube Config

kubectl config get-contexts
kubectl config current-context
kubectl config use-context gcpcluster-k8s-1
	Switched to context "gcpcluster-k8s-1"
--
Deploy microservices to AKS

kubectl apply -f .\aks\
--
Clean All AKS and Azure Resources

az group delete --name myResourceGroup --yes --no-wait
