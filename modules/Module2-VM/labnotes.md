# Virtual Machines - Lab Notes
# Module: Describe the core architectural components of Azure

Goal: 
Create a virtual machine > Install Nginx > Access web server > List current security group rules > Create the network security rule > Access web server again

Why is this important? 

Task 1: Create a resource group using Cloud shell
1. Logged into Azure and opened up Cloud shell (Azure CLI)
2. Opened up Cloud shell from the top right corner next to your profile and typed in:

```CLI
az group create 
--name MinuVirtukas 
--location eastus
```
3. With that we created a resource group named MinuVirtukas (see 1.png)

Task 2: Create a Linux virtual machine
1. From the cloud shell we run the command "az vm create" command to create Linux VM:
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
We can read from this that SKU Standard_D2s_v5 is unavailable in US east region. So for a workaround I decided to deviate from the learn path and instead use the region closest to me which was to be Norway East.
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
This command gave as an output of all the available SKUS in norwayeast region and in it I could see which ones I could use and which ones I couldn't due to subsrciption restrictions. (see 2.png)

However the list was a bit too long and uneasy for the eyes for me to properly filter out, additionally it showed me all the SKUs but I needed Standard_D2s speficically. 

So a bit of googling and forum reading I found a commandline that would filter out all the available SKUs available in the region I was using as well as filter out all the Standard_D2s variants
```CLI
az vm list-skus --location centralus --size Standard_D --all --output table
```
I modified the commandline to filter out specifically Standard_D2s and additionally to leave out all the SKUs that werent available for my subscription
```CLI
az vm list-skus --location norwayeast --size Standard_D2s --all false --output table
```
With that commandline I managed to go from 100s of rows to pick from to just 4 (see 3.png)
