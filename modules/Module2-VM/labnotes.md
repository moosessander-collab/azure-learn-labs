# Virtual Machines - Lab Notes
# Module: Describe the core architectural components of Azure

Goal: 
Create a virtual machine > Install Nginx > Access web server > List current security group rules > Create the network security rule > Access web server again

Why is this important? 

Task 1: Create a resource group using Cloud shell
1. Logged into Azure and opened up Cloud shell (Azure CLI)
2. Opened up Cloud shell from the top right corner next to your profile and typed in:
    az group create --name MinuVirtukas --location eastus
3. With that we created a resource group named MinuVirtukas (see 1.png)

Task 2: Create a Linux virtual machine
1. From the cloud shell we run the command "az vm create" command to create Linux VM


az vm create
--resource-group MinuVirtukas  
--name my-vm  
--size Standard_D2s_v5  
--public-ip-sku Standard  
--image Ubuntu2204  
--admin-username virtualhermit  
--generate-ssh-keys
   
