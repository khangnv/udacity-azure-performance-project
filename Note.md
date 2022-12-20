## Sample source code

https://github.com/udacity/nd081-c4-azure-performance-project-starter

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
7. Clode code
   git clone https://github.com/khangnv/udacity-azure-performance-project.git
   cd udacity-azure-performance-project
   git checkout Deploy_to_VMSS
