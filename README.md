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
   - Google Wireshark and install the Windows x64 version.
   - Hit next through all the default install steps.

      ![Screenshot 2024-03-07 165253](https://github.com/d-lowes/azure-network-protocols/assets/121920809/a7684171-0acc-4d52-be37-355c474d0b8a)
   - Open Wireshark and click the blue fin logo in the top left corner while the Ethernet row is highlighted.
![Screenshot 2024-03-07 165623](https://github.com/d-lowes/azure-network-protocols/assets/121920809/30d9f5ab-6c5d-46af-ab1f-a499e4a5c302)
   - Traffic should be spamming rapidly!
![Screenshot 2024-03-07 165814](https://github.com/d-lowes/azure-network-protocols/assets/121920809/11242b8e-af79-4437-aeaf-1212984dcdde)

3. **Capture ICMP Traffic:**
   - Open Wireshark and filter for ICMP traffic only. (Type icmp in the search bar and hit enter twice. Traffic should stop spamming)
   ![Screenshot 2024-03-07 170930](https://github.com/d-lowes/azure-network-protocols/assets/121920809/f816cc59-9e8f-49dd-9045-4929f30a456d)
   - Open the command line or powershell on your windows VM and ping the Ubuntu VM's Private IP address to observe the requests and replies in Wireshark (yours may be different).
![Screenshot 2024-03-07 170544](https://github.com/d-lowes/azure-network-protocols/assets/121920809/d6e5a842-7241-443c-bad4-a41e053e571b)
![Screenshot 2024-03-07 171156](https://github.com/d-lowes/azure-network-protocols/assets/121920809/197ac520-8329-4f34-b550-b9a08eb559cd)
   - Try pinging a public website (e.g., www.google.com) from the Windows VM and observe the traffic.
![Screenshot 2024-03-07 171239](https://github.com/d-lowes/azure-network-protocols/assets/121920809/bff9427b-d482-41c5-8b43-c1850d2e35d7)

4. **Modify ICMP Traffic Rules:**
   - Initiate a continuous ping to your Ubuntu VM by adding the `-t` flag after the IP address in your ping command.
![Screenshot 2024-03-07 171721](https://github.com/d-lowes/azure-network-protocols/assets/121920809/08d77870-fbb8-4868-ab02-6fe3e313d5c0)

   - Navigate to your Ubuntu VM's network security group and select `Inbound security rules` on the left tab.
![Screenshot 2024-03-07 172037](https://github.com/d-lowes/azure-network-protocols/assets/121920809/bd347f63-c37a-4cf8-a5c5-9f3ad6b170ed)

   - Add the Network Security Group for the Ubuntu VM to disable incoming ICMP traffic.
![Screenshot 2024-03-07 172308](https://github.com/d-lowes/azure-network-protocols/assets/121920809/a75abeec-c878-4f47-ae8b-c1b531385d3f)

   - Observe the change in ping responses and Wireshark traffic.
![Screenshot 2024-03-07 172403](https://github.com/d-lowes/azure-network-protocols/assets/121920809/350a4184-698d-42e3-a7c7-09b22e10b093)

   - Re-enable ICMP traffic in the NSG and observe the traffic resume.
![Screenshot 2024-03-07 172459](https://github.com/d-lowes/azure-network-protocols/assets/121920809/dca21fe4-3860-45a9-b632-ee9e7a7b4a76)

#### **Observing SSH Traffic**

1. **Filter for SSH Traffic:**
   - Change the Wireshark filter to SSH traffic only.

2. **SSH into Ubuntu VM:**
   - Run Windows PowerShell as an administrator.
![Screenshot 2024-03-07 172722](https://github.com/d-lowes/azure-network-protocols/assets/121920809/15e5c8fc-f10c-4ade-a00b-788e0e8f6a1f)

   - From the Windows VM, use an SSH client to connect to the Ubuntu VM by typing the command `ssh user@ip-address` replacing user and ip-address with the username of the Ubuntu VM and its private IP address.
   - Observe the SSH traffic in Wireshark as you execute commands.
![Screenshot 2024-03-07 173056](https://github.com/d-lowes/azure-network-protocols/assets/121920809/a8ad7817-7b9a-4820-8b2e-e13c6f23442b)
![Screenshot 2024-03-07 173151](https://github.com/d-lowes/azure-network-protocols/assets/121920809/7c7dbeec-3a29-46f3-b73a-8dc455c6b148)

3. **Exit SSH Session:**
   - Type `exit` and observe the cessation of SSH traffic in Wireshark.

**Observing DHCP Traffic**

1. **Filter for DHCP Traffic:**
   - Set Wireshark to display DHCP traffic only.

2. **Renew IP Address:**
   - Issue an `ipconfig /renew` command from the Windows VM and observe the DHCP traffic in Wireshark.
![Screenshot 2024-03-07 173342](https://github.com/d-lowes/azure-network-protocols/assets/121920809/a931a5cb-b8e6-49e6-8e94-b52459e0538c)

#### **Observing DNS Traffic**

1. **Filter for DNS Traffic:**
   - Adjust Wireshark to filter DNS traffic only.
   - Clear the current window to make a fresh slate `restart current capture`. Click `continue without saving`.
![Screenshot 2024-03-07 173446](https://github.com/d-lowes/azure-network-protocols/assets/121920809/cc507d5e-3acf-4f15-b868-77202260f790)

2. **Use nslookup:**
   - Perform `nslookup` for domains like google.com and disney.com from the Windows VM and watch the DNS queries in Wireshark.
![Screenshot 2024-03-07 173656](https://github.com/d-lowes/azure-network-protocols/assets/121920809/a67af9f7-b6e2-4818-807e-355614603ce0)

##### **Observing RDP Traffic**

1. **Filter for RDP Traffic:**
   - Apply a Wireshark filter for RDP traffic (`tcp.port == 3389`).
   - Before hitting enter, what do you think will happen?
   - Considering we are connecting to a remote desktop..Notice the continuous flow of RDP traffic, which is due to the live streaming nature of the RDP protocol.
![Screenshot 2024-03-07 174028](https://github.com/d-lowes/azure-network-protocols/assets/121920809/415501bd-13ad-4436-938c-04343d425813)

## Important!
### Lab Cleanup

1. **Close Remote Desktop Connection:**
   - Ensure you log off from your Windows VM.

      ![Screenshot 2024-03-07 174100](https://github.com/d-lowes/azure-network-protocols/assets/121920809/a9c044bd-b992-4cdf-bea6-67673bf463cd)

2. **Delete Resource Groups:**
   - Navigate to Resource Groups in the Azure Portal.
   - Delete the Resource Group(s) you created for this lab.
![Screenshot 2024-03-07 174143](https://github.com/d-lowes/azure-network-protocols/assets/121920809/6f0d1b75-7365-41f3-82d1-78a20b09272a)
   - Delete all resources including N
   - Confirm the deletion to ensure no resources are left running.

By following these steps, you will gain hands-on experience with network traffic types and understand how NSG rules can control traffic flow in an Azure environment. This lab also highlights the practical use of Wireshark for network analysis and troubleshooting.
