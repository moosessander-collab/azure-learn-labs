# Module: Describe the core architectural components of Azure
# Configuring network access for installed VM and accessing web server - Lab Notes

Goal: In this exercise I needed to configure network access to the Virtual machine I had created prior in the last exercise.

**Note!** This exercise required me to use BASH version of cloud shell for some of the commands.

### Task 1: Accessing my web server

1. For starters I need to access web server that I had installed prior to this exercise. To start things off I used ```az vm list-ip-addresses``` command to get my VMs IP address and store the result as a Bash variable:

#### Command: List IP addresses
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

#### Command: Connection testing
```bash
curl --connect-timeout 5 http://$IPADDRESS
```
We were met with the following error ```curl: (28) Connection timed out after 5001 milliseconds```

Why? Because I didnt create any network security roles yet.

4. As an option I tried to access web server from the browser by running the command ```echo $IPADDRESS```.

With that I could copy Virtual machines IP address and copy paste it into URL for the same result just visible from the browser [unreachable.png](../screenshots/unreachable.PNG) 

So there are 2 ways to ping whether VMs web server is up or not.

### Task 2: Listing the current network security group rules

1. Next I ran the command ```az network nsg list``` to see what NSG was associated with my VM

#### Command: NSG name
```bash
az network nsg list \
--resource-group "MinuVirtukas" \
--query '[].name' \
--output tsv
```
Output ```minu-virtukasNSG``` 

**Note!** By default azure VMs have at least one NSG. If you leave the default name for vm its going to be ```my-vmNSG```. But that is not my case.

2. Now I needed to list the rules associated with the NSG:

#### Command: NSG rule list
```bash
az network nsg rule list \
--resource-group MinuVirtukas \
--nsg-name minu-virtukasNSG
```
This prints out a large block of JSON which im not satisfied with because I needed specifically: name, priority, affected ports and access for each rule.

So I ran the ```az network nsg rule list``` again:

#### Command: NSG rule list
```bash
az network nsg rule list \
--resource-group MinuVirtukas \
--nsg-name minu-virtukasNSG \ 
--query "[].{Name:name, Priority:priority, Port:destinationPortRange, Access:acess}" \
--output table
```
Output:
```bash
Name               Priority    Port    Access
-----------------  ----------  ------  --------
default-allow-ssh  1000        22      Allow
```

**Note!** By default Linxu VMs NSG allows network access only on port 22.

Next I needed to also allow inbound connections on port 80 to access over HTTP.

### Task 3: Create NSG rule

1. Ran the following ```az network nsg rule create```

#### Command: NSG rule create
```bash
az network nsg rule create \
--resource-group MinuVirtukas \
--nsg-name minu-virtukasNSG \
--name allow-http \
--protocol tcp \
--priority 100 \
--destination-port-range 80 \
--access Allow
```
**Note!** For learning purposes I set the priority to 100 as instructed.

Output: [nsgrule.png](../screenshots/nsgrule.PNG) 

Then I wanted to verify the configuration by using the same command I used in Task 2: [Command: NSG rule list](#command-nsg-rule-list)





