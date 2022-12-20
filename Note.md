## Sample source code

https://github.com/udacity/nd081-c4-azure-performance-project-starter
https://github.com/LaurentVeyssier/nd081-c4-azure-performance-project-starter/tree/master

## Create resources

1. `az login`
2. Open bash terminal => run `./setup-script.sh`
3. Create Application Insights
4. Enable Application Insights monitoring for the VM Scale Set.
   Copy the `Instrumentation Key` of the Application Insights instance to use in the `main.py` next.
5. Find the port for connecting via SSH
   az vmss list-instance-connection-info \
    --resource-group khangnv2-rg \
    --name udacity-vmss
6. Connect to VM
   ssh -p [port number] udacityadmin@[public-ip]
7. Clone code
   git clone https://github.com/khangnv/udacity-azure-performance-project.git
   cd udacity-azure-performance-project
   git checkout Deploy_to_VMSS

## AKS

# Change the ACR name in the commands below.

# Assuming the khangnv2-rg resource group is still available with you

# ACR name should not have upper case letter

az acr create --resource-group khangnv2-rg --name khangnv2acr --sku Basic

# Log in to the ACR

az acr login --name khangnv2acr

# Get the ACR login server name

# To use the azure-vote-front container image with ACR, the image needs to be tagged with the login server address of your registry.

# Find the login server address of your registry

az acr show --name khangnv2acr --query loginServer --output table

# Associate a tag to the local image. You can use a different tag (say v2, v3, v4, ....) everytime you edit the underlying image.

docker tag azure-vote-front:v1 khangnv2acr.azurecr.io/azure-vote-front:v1

# Now you will see khangnv2acr.azurecr.io/azure-vote-front:v1 if you run "docker images"

# Push the local registry to remote ACR

docker push khangnv2acr.azurecr.io/azure-vote-front:v1

# Verify if your image is up in the cloud.

az acr repository list --name khangnv2acr --output table

# Associate the AKS cluster with the ACR

az aks update -n udacity-cluster -g khangnv2-rg --attach-acr khangnv2acr

# Get the ACR login server name

az acr show --name khangnv2acr --query loginServer --output table

# Deploy the application. Run the command below from the parent directory where the _azure-vote-all-in-one-redis.yaml_ file is present.

kubectl apply -f azure-vote-all-in-one-redis.yaml
kubectl set image deployment azure-vote-front azure-vote-front=khangnv2acr.azurecr.io/azure-vote-front:v1

# Test the application at the External IP

# It will take a few minutes to come alive.

kubectl get service

# Check the status of each node

kubectl get pods

# Push your local changes to the remote Github repo, preferably in the Deploy_to_AKS branch
