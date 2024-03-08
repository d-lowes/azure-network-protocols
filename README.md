<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
This tutorial walks you through the steps of setting up a virtual network environment in Azure, including Windows 10 and Ubuntu virtual machines (VMs). You will observe and analyze different types of network traffic using Wireshark, such as ICMP, SSH, DHCP, DNS, and RDP, to understand their characteristics and how Azure Network Security Groups (NSGs) control the flow of traffic.
<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create Resources
- Set Up the Environment
- Observe Network Traffic
- Cleanup Resources

<h2>Actions and Observations</h2>

### **Set Up the Environment**

1. **Create a Resource Group:**
   - Log into the Azure Portal.
   - Navigate to Resource Groups and create a new one, specifying a name and region.
![Screenshot 2024-03-07 160822](https://github.com/d-lowes/azure-network-protocols/assets/121920809/63d7eeb8-99a1-4790-9ce5-22a7ebf6f3f1)

2. **Create a Windows 10 Virtual Machine:**
   - Within the created Resource Group, create a new Windows 10 VM.
   - Use 2 vcpus and ensure that authentication type is "password", then proceed to create a username and password.
   - Allow Azure to create a new Virtual Network (Vnet) and Subnet for this VM.
![Screenshot 2024-03-07 160419](https://github.com/d-lowes/azure-network-protocols/assets/121920809/1b8505f0-b252-4f6d-9ceb-09b3b2a148d7)

   - Your resource group will look like this after creating your Windows virtual machine (note the vnet name):
![Screenshot 2024-03-07 160741](https://github.com/d-lowes/azure-network-protocols/assets/121920809/4d4b0e82-7807-464e-8e8f-0128abd1ddb5)

3. **Create a Linux (Ubuntu) VM:**
   - Within the same Resource Group, create an Ubuntu VM.
   - Use 2 vcpus and ensure that authentication type is "password", then proceed to create a username and password.
![Screenshot 2024-03-07 161010](https://github.com/d-lowes/azure-network-protocols/assets/121920809/13b45726-472a-46b1-9851-a21df934ca14)
![Screenshot 2024-03-07 161023](https://github.com/d-lowes/azure-network-protocols/assets/121920809/8fd302c8-d388-4b2c-a024-eafd6c41db0e)
   - Use the existing vnet created for the Windows VM.
![Screenshot 2024-03-07 163104](https://github.com/d-lowes/azure-network-protocols/assets/121920809/9906561e-e7c9-45ad-b74b-8ffcb7ebc07b)
   - Your resource group will look like this after creating your Ubuntu virtual machine (sort by type):
![Screenshot 2024-03-07 163708](https://github.com/d-lowes/azure-network-protocols/assets/121920809/db5e3d9f-d91c-40f6-ba9e-54e3eb2f5df2)


4. **Observe Your Virtual Network:**
   - Navigate to Network Watcher and observe your Vnet's topology, including both VMs.

### Observe Network Traffic


#### **Observing ICMP Traffic**

1. **Connect to Your Windows VM via Remote Desktop and Install Wireshark.**
   - Copy the public IP address and paste it into Remote Desktop Protocol (search RDP in Windows) and type your saved login info from earlier.
![Screenshot 2024-03-07 164329](https://github.com/d-lowes/azure-network-protocols/assets/121920809/20f6eb25-ab23-4ecf-89bf-b8a37e0e05d2)
![Screenshot 2024-03-07 164555](https://github.com/d-lowes/azure-network-protocols/assets/121920809/01be7c75-a24d-401f-b2be-afd174173e7d)
![Screenshot 2024-03-07 164530](https://github.com/d-lowes/azure-network-protocols/assets/121920809/60ac643d-57e7-44a2-8cc6-d362d43c401f)

3. **Capture ICMP Traffic:**
   - Open Wireshark and filter for ICMP traffic only.
   - Open the command line or powershell on your windows VM and ping the Ubuntu VM to observe the requests and replies in Wireshark.
   - Try pinging a public website (e.g., www.google.com) from the Windows VM and observe the traffic.

4. **Modify ICMP Traffic Rules:**
   - Initiate a continuous ping to your Ubuntu VM.
   - Edit the Network Security Group for the Ubuntu VM to disable incoming ICMP traffic.
   - Observe the change in ping responses and Wireshark traffic.
   - Re-enable ICMP traffic in the NSG and observe the traffic resume.

#### **Observing SSH Traffic**

1. **Filter for SSH Traffic:**
   - Change the Wireshark filter to SSH traffic only.

2. **SSH into Ubuntu VM:**
   - From the Windows VM, use an SSH client to connect to the Ubuntu VM by typing the command `ssh user@ip-address` replacing user and ip-address with the username of the Ubuntu VM and its private IP address.
   - Observe the SSH traffic in Wireshark as you execute commands.

3. **Exit SSH Session:**
   - Type `exit` and observe the cessation of SSH traffic in Wireshark.

**Observing DHCP Traffic**

1. **Filter for DHCP Traffic:**
   - Set Wireshark to display DHCP traffic only.

2. **Renew IP Address:**
   - Issue an `ipconfig /renew` command from the Windows VM and observe the DHCP traffic in Wireshark.

#### **Observing DNS Traffic**

1. **Filter for DNS Traffic:**
   - Adjust Wireshark to filter DNS traffic only.

2. **Use nslookup:**
   - Perform `nslookup` for domains like google.com and disney.com from the Windows VM and watch the DNS queries in Wireshark.

##### **Observing RDP Traffic**

1. **Filter for RDP Traffic:**
   - Apply a Wireshark filter for RDP traffic (`tcp.port == 3389`).
   - Notice the continuous flow of RDP traffic, which is due to the live streaming nature of the RDP protocol.

## Important!
### Lab Cleanup

1. **Close Remote Desktop Connection:**
   - Ensure you log off from your Windows VM.

2. **Delete Resource Groups:**
   - Navigate to Resource Groups in the Azure Portal.
   - Delete the Resource Group(s) you created for this lab.
   - Confirm the deletion to ensure no resources are left running.

By following these steps, you will gain hands-on experience with network traffic types and understand how NSG rules can control traffic flow in an Azure environment. This lab also highlights the practical use of Wireshark for network analysis and troubleshooting.
