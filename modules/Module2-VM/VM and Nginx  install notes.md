# Module: Describe the core architectural components of Azure
# Virtual machine creation and Nginx installation - Lab Notes


Goal: 
Create a virtual machine > Install Nginx 
 

Task 1: Create a resource group using Cloud shell
1. Logged into Azure and opened up Cloud shell (Azure CLI)
2. Opened up Cloud shell from the top right corner next to my profile and typed in:

```CLI
az group create 
--name MinuVirtukas 
--location eastus
```
3. With that I created a resource group named MinuVirtukas (see resourcegrp.png)

Task 2: Create a Linux virtual machine
1. From the cloud shell I ran the command "az vm create" command to create Linux VM:
```CLI
az vm create
--resource-group MinuVirtukas  
--name my-vm  
--size Standard_D2s_v5  
--public-ip-sku Standard  
--image Ubuntu2204  
--admin-username virtualhermit  
--generate-ssh-keys
```
2. Ran into the error and fixed it

```CLI
Message: The requested VM size for resource 'Following SKUs have failed for Capacity Restrictions: Standard_D2s_v5' is currently not available in location 'eastus'. Please try another size or deploy to a different location or different zone. See https://aka.ms/azureskunotavailable for details.
```
I could read from this that SKU Standard_D2s_v5 is unavailable in US east region. So for a workaround I decided to deviate from the learn path and instead use the region closest to me which was to be Norway East.
But before I continued I needed to delete the group I created prior.

To do that I used the following command
```CLI
az group delete --name MinuVirtukas
```
Before I reverted back to task1 commandline I needed to know whether norway east region had the SKU needed to perform the task, and if not the exact same one as described in the learning path then at least an alternative that would work. The learning path wanted me to use Standard_D2s_v5 so I had to keep in mind that it either needed to be exact same or at least Standard_D

To do that I used the command line
```CLI
az vm list-skus --location norwayeast --output table
```
This command gave me an output of all the available SKUS in norwayeast region and in it I could see which ones I could use and which ones I couldn't due to subsrciption restrictions. (see skulist.png)

However the list was a bit too long and uneasy for the eyes for me to properly filter out, additionally it showed me all the SKUs but I needed Standard_D2s speficically. 

So a bit of googling and forum reading I found a commandline that would filter out all the available SKUs available in the region.
```CLI
az vm list-skus --location centralus --size Standard_D --all --output table
```
However I modified the commandline to filter out specifically Standard_D2s and additionally to leave out all the SKUs that werent available for my subscription
```CLI
az vm list-skus --location norwayeast --size Standard_D2s --all false --output table
```
With that commandline I managed to go from 100s of rows to pick from to just 4 (see filteredskus.png)
The end result was that Standard_D2s_v5 was in fact available in this region but I wanted to know how to filter SKUs with ease for my own benefit, this step was easily skippable in hindsight, however I wanted to challenge my self and troubleshoot because that way what im doing I will also remember. Trial and error.

Task 2: Create a Linux VM

Went back to task 1  and created a new resource group again, this time with a region that had the SKU that I needed
```CLI
az group create --name MinuVirtukas --location norwayeast
```
4. Once the region was set and resource group created, I went and created the VM itself
```CLI
az vm create
--resource-group MinuVirtukas
--name minu-virtukas
--size Standard_D2s_v5
--public-ip-sku Standard
--image Ubuntu2204
--admin-username virtualhermit
--generate-ssh-keys
```
With that out of the way I could move on and install Nginx (for the result see VMcreation.png) *P.S I do apologize for the 4.png if it looks confusing at first, Azures CLI is clunky at times*

Task 3: Install Nginx

Once I had my VM created I needed to use Custom script extension to install Nginx. For that we needed to run az vm extension set command to configure Nginx to my VM
```CLI
az vm extension set
--resource-group MinuVirtukas
--vm-name minu-virtukas
--name customScript 
--publisher Microsoft.Azure.Extensions
--version 2.1
--settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"]}' 
--protected-settings '{"commandToExecute": "./configure-nginx.sh"}'
```
With that out of the way I had finally installed Nginx (see Nginx.png)

References:

https://learn.microsoft.com/en-us/training/modules/describe-azure-compute-networking-services/

https://learn.microsoft.com/en-us/azure/azure-resource-manager/troubleshooting/error-sku-not-available?tabs=azure-cli
