# Module: Describe the core architectural components of Azure
# Virtual Machine creatin in Azure interface - Lab Notes

Goal: Create a virtual machine in Microsoft Azure

Why is this important? VM deployment is the foundation for IaaS workloads

## Steps I took
1. Made a trial account for Microsoft Azure
2. Selected "Virtual machines" option from Azure services menu
3. From the upperside menu picked "+create" > "Virtual machine"
4. Verified and entered the followin values provided by the learn path as following:
Subscription - default (if using trial version then shows up as "Azure subscription 1"
Resource group - Lab told me to name it IntroAzureRG but I named it instead "Virtukas"
Virtual machine - named it "my-virtukas"
Region - left default which was "Norway East"
SSH - ports left open to internet for testin reasons
Username - azurehermit
The rest - left default
5. Since the goal was to simply create a virtual machine then no other tabs needed to be specified so I went ahead and hit "Review + create"
6. Waited for Validation of VM which took around 30sec, went ahead and pressed "Create"
7. VM deployment took around 20 sec, deployment complete

No issues in creating the VM since it is pretty straight forward and this is pretty basic.

After the VM was successfully deployed I went back to home page and scrolled down to a section that is called "Navigate".
From there I selected "Resource Groups" since the purpose of this task was to create a resource i.e the VM and we want to check whether we succeeded. We did.

I have provided screenshots in the Module1-VM folder for further proof.

Congrats! You now know how to deploy a VM in Microsoft Azure! Juhhuu!

References: https://learn.microsoft.com/en-us/training/modules/describe-core-architectural-components-of-azure/
