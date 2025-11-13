# Module: Describe the core architectural components of Azure
# Configuring network access for installed VM and accessing web server - Lab Notes

Goal: In this exercise I needed to configure network access to the Virtual machine I had created prior in the last exercise.

**Note:** This exercise required me to use BASH version of cloud shell for some of the commands.

Task 1: Accessing my web server

1. For starters I need to access web server that I had installed prior to this exercise. To start things off I used ```az vm list-ip-addresses``` command to get my VMs IP address and store the result as a Bash variable:
```bash
IPADDRESS="$(az vm list-ip-addresses \
--resource-group "MinuVirtukas" \
--name my-vm \
--query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
--output tsv)"
```
2. Ran into the error [error.png](../screenshots/error.PNG), instead of ```--resource-group``` I miss spelled it in my haste to ```--resource-grp```

After fixing my GRAVE error the command went through [fix.png](../screenshots/fix.PNG)

3. Now I needed to download the home page to see if the VM is reachable and whether I could access the webserver (Spoiler alert it wasnt and I couldnt for obvious reasons):
```bash
curl --connect-timeout 5 http://$IPADDRESS
```
We were met with the following error ```curl: (28) Connection timed out after 5001 milliseconds```

Why? Because I didnt create any network security roles yet.

4. As an option I tried to access web server from the browser by running the command ```echo $IPADDRESS```.

With that I could copy Virtual machines IP address and copy paste it into URL for the same result just visible from the browser [unreachable.png](../screenshots/unreachable.PNG) 

So there are 2 ways to ping whether VMs web server is up or not.

Task 2: Listing the current network security group rules

To find out why the web server wasnt accessible I checked current NSG rules with ```az network nsg list```

```bash
az network nsg list \
--resource-group "MinuVirtukas" \
--query '[].name' \
--output tsv
```
Ran into another error [error2.png](../screenshots/error2.PNG) 
In the lab it is mentioned that every VM on Azure is associated with at least one network security group but what they failed to mention was that the name of it is not always defaulted to *my-vmNSG*.

To fix it I ran the following command to see the name of mine:
```bash
az network nsg list --resource-group MinuVirtukas --output table
```
The result shocked me [nsg.png](../screenshots/nsg.PNG). Of course it was something that obvious in hinsight.








