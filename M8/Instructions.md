
# Lab: Configuring VNet peering and service chaining
  

### Lab Setup
  
Estimated Time: 45 minutes

User Name: **Student**

Password: **Pa55w.rd**


## Exercise 1: Creating the vms
 

#### Task 1: Create the vms

1. From the Cloud Shell pane, open a bash command line and run the script shown below

Please Check this script [Create Resources](https://raw.githubusercontent.com/cemvarol/AZ-301/master/T4M3/NewLab/CreateResources)

## Exercise 2: Configuring VNet peering 

  
The task for this exercise is as follows:

1. Configure VNet peering for both virtual networks

#### Task 1: Configure VNet peering for both virtual networks
  
1. In the Microsoft Edge window displaying the Azure portal, navigate to the **az3000401-vnet** virtual network blade.

1. From the **az3000401-vnet** blade, create a VNet peering with the following settings:

    - Name of the peering from the first virtual network to the second virtual network: **az3000401-vnet-to-az3000402-vnet**

    - Virtual network deployment model: **Resource manager**

    - Subscription: the name of the Azure subscription you are using for this lab

    - Virtual network: **az3000402-vnet**
    
    - Name of the peering from the second virtual network to the first virtual network: **az3000402-vnet-to-az3000401-vnet**    
    
    - Allow virtual network access from the first virtual network to the second virtual nework: **Enabled**
    
    - Allow virtual network access from the second virtual network to the first virtual nework: **Enabled**    

    - Allow forwarded traffic from the first virtual network to the second virtual network: **Disabled**
    
    - Allow forwarded traffic from the second virtual network to the first virtual network: **Disabled**

    - Allow gateway transit: disabled

## Exercise 3: Implementing routing
  
The main tasks for this exercise are as follows:

1. Enable IP forwarding

2. Configure user defined routing 

3. Configure routing on an Azure VM running Windows Server 2016


#### Task 1: Enable IP forwarding 
  
1. In Microsoft Edge, navigate to the **az3000401-vm2-RVMNic** blade (the NIC of **az3000401-vm2-R**)

2. On the **az3000401-vm2-RVMNic** blade, modify the **IP configurations** by setting **IP forwarding** to **Enabled**.


#### Task 2: Configure user defined routing 

1. In the Azure portal, create a new route table with the following settings:

    - Name: **az3000402-rt1**

    - Subscription: the name of the Azure subscription you use for this lab

    - Resource group: **az3000402-LabRG**

    - Location: the same Azure region in which you created the virtual networks
  
    - Virtual network gateway route propagation: **Disabled**
    
    Once the creation of the route table has finished, click on **Go to resource**

2. In the Azure portal, on the route table az3000402-rt1 that was created on the previous step, click on **Routes** under **Settings** and add a route with the following settings: 

    - Route name: **custom-route-to-az3000401-vnet**

    - Address prefix: **10.0.0.0/22**

    - Next hop type: **Virtual appliance**

    - Next hop address: **10.0.1.4**

3. In the Azure portal, associate the route table with the **subnet-1** of the **az3000402-vnet**.


#### Task 3: Configure routing on an Azure VM running Windows Server 2016

1. On the lab computer, from the Azure portal, start a Remote Desktop session to **az3000401-vm2-R** Azure VM. 

2. When prompted to authenticate, specify the following credentials:

    - User name: **Student**

    - Password: **Pa55w.rd1234**

3. Once you are connected to az3000401-vm2-R via the Remote Desktop session, 
Run these commands below on **powershell** console of that VM

---
 
**Set-NetFirewallProfile -Enabled False**

**Install-WindowsFeature Routing, RSAT-RemoteAccess**

**rrasmgmt.msc**

----

4. Follow the steps to complete RRAS setting... Check this [image](https://github.com/cemvarol/AZ-301/blob/master/T4M3/NewLab/RouterConfig.png)



> **Result**: After completing this exercise, you should have configured custom routing within the second virtual network.


## Exercise 4: Validating service chaining
  
The main tasks for this exercise are as follows:

1. Configure Windows Firewall with Advanced Security on an Azure VM

1. Test service chaining between peered virtual networks


#### Task 1: Configure Windows Firewall with Advanced Security on the target Azure VMs
  
1. On the lab computer, from the Azure portal, start a Remote Desktop session to **az3000401-vm1** Azure VM. 

2. When prompted to authenticate, specify the following credentials:

    - User name: **Student**

    - Password: **Pa55w.rd1234**

3. In the Remote Desktop session to az3000401-vm1, run the **powershell** command below on that vm

**Set-NetFirewallProfile -Enabled False**

#### Task 2: Test service chaining between peered virtual networks
  
1. On the lab computer, from the Azure portal, start a Remote Desktop session to **az3000402-vm1** Azure VM. 

1. When prompted to authenticate, specify the following credentials:

    - User name: **Student**

    - Password: **Pa55w.rd1234**

1. Once you are connected to az3000402-vm1 via the Remote Desktop session, start **Windows PowerShell**.

1. In the **Windows PowerShell** window, run the following:

   ```pwsh
   Set-NetFirewallProfile -Enabled False
   
   Test-NetConnection -ComputerName 10.0.0.4 -TraceRoute
   ```

1. Verify that test is successful and note that the connection was routed over 10.0.1.4

>  **Result**: After completing this exercise, you should have validated service chaining between peered virtual networks.

## Exercise 5: Remove lab resources


1. At the **Cloud Shell** command prompt, type in the following command and press **Enter** to delete all resource groups you created in this lab:


   ```sh
   az group list --query "[?starts_with(name,'az30004')]".name --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```

1. Close the **Cloud Shell** prompt at the bottom of the portal.

> **Result**: In this exercise, you removed the resources used in this lab.
