# Azure Virtual Network Traffic Observation Lab

## Lab Overview

This lab walks through the process of creating Windows and Linux virtual machines (VMs) in Microsoft Azure, connecting them to the same virtual network, and using Wireshark to monitor various types of network traffic. You'll observe ICMP, SSH, DHCP, DNS, and RDP protocols in action while also modifying Network Security Group (NSG) rules to test traffic filtering.

---

## Technologies Used

- **Microsoft Azure** – Cloud platform used to deploy VMs and networking resources
- **Windows 10** and **Ubuntu Linux** – Operating systems used for the lab environment
- **Wireshark** – Network protocol analyzer used to inspect live traffic
- **Remote Desktop Protocol (RDP)** – Used to access the Windows VM
- **PowerShell / Command Prompt** – Used for network operations and testing
- **Network Security Groups (NSG)** – Azure firewall tool to control inbound and outbound traffic

---

## Part 1 – Create our Resources

1. **Create a Resource Group**
   - Go to the [Azure Portal](https://portal.azure.com/)
   - Create a new Resource Group to contain all lab resources

2. **Create a Windows 10 Virtual Machine (VM)**
   - Select the previously created Resource Group
   - Allow Azure to create a new Virtual Network (VNet) and Subnet
   - Use Username/Password authentication

3. **Create a Linux (Ubuntu) Virtual Machine**
   - Select the same Resource Group used above
   - Select the same VNet created with the Windows VM
   - Use Username/Password authentication

4. **Observe Your Virtual Network in Network Watcher**
   - Navigate to **Network Watcher** in Azure
   - Explore the topology of your Virtual Network, observing both VMs connected to the same Subnet

---

## Part 2 – Observe ICMP Traffic

1. **Connect to Windows VM via Remote Desktop**
   - Use RDP with the public IP of your Windows VM

2. **Install Wireshark on the Windows VM**
   - Download and install Wireshark from [https://www.wireshark.org](https://www.wireshark.org)

3. **Filter for ICMP Traffic in Wireshark**
   - Open Wireshark
   - Start capturing on the main network interface
   - Apply the filter: `icmp`

4. **Ping Ubuntu VM from Windows VM**
   - Retrieve the **private IP address** of the Ubuntu VM from Azure
   - Open PowerShell or CMD and run:
     ```bash
     ping <ubuntu_private_ip>
     ```
   - Observe ICMP requests and replies in Wireshark

5. **Ping a Public Website from Windows VM**
   - Run:
     ```bash
     ping www.google.com
     ```
   - Observe public ICMP traffic in Wireshark

6. **Start a Continuous Ping to Ubuntu VM**
   - Run:
     ```bash
     ping <ubuntu_private_ip> -t
     ```

7. **Disable Inbound ICMP in Ubuntu VM's NSG**
   - Go to the **Network Security Group** attached to your Ubuntu VM’s NIC or subnet
   - Add a rule to **deny inbound ICMP traffic**

8. **Observe Blocked ICMP Traffic**
   - Wireshark will show repeated ICMP requests without replies
   - PowerShell ping will show timeout or unreachable messages

9. **Re-enable Inbound ICMP in NSG**
   - Remove or disable the deny rule
   - Observe successful ping replies resuming in Wireshark and PowerShell

10. **Stop the Ping Activity**
    - Press `Ctrl + C` to stop the ping command

---

## Part 3 – Observe SSH Traffic

1. **Filter for SSH in Wireshark**
   - Apply the filter: `ssh`

2. **SSH into the Ubuntu VM**
   - In PowerShell, run:
     ```bash
     ssh <username>@<ubuntu_private_ip>
     ```
   - Enter the password when prompted
   - Type a few basic Linux commands (e.g., `ls`, `pwd`)

3. **Observe SSH Traffic in Wireshark**
   - Watch encrypted packet flow as you interact with the Ubuntu VM

4. **Exit the SSH Session**
   - Type `exit` and press Enter

---

## Part 4 – Observe DHCP Traffic

1. **Filter for DHCP in Wireshark**
   - Apply the filter: `DHCP`

2. **Renew the IP Address**
   - Open PowerShell as Administrator
   - Run:
     ```bash
     ipconfig /renew
     ```

3. **Observe DHCP Traffic**
   - Watch DHCP discovery, offer, request, and acknowledgment messages in Wireshark

---

## Part 5 – Observe DNS Traffic

1. **Filter for DNS in Wireshark**
   - Apply the filter: `dns`

2. **Use nslookup to Resolve Domains**
   - In PowerShell, run:
     ```bash
     nslookup google.com
     nslookup disney.com
     ```

3. **Observe DNS Traffic**
   - Watch DNS query and response packets in Wireshark

---

## Part 6 – Observe RDP Traffic

1. **Filter for RDP in Wireshark**
   - Apply the filter: `tcp.port == 3389`

2. **Observe Continuous RDP Flow**
   - Wireshark will display nonstop traffic from the ongoing RDP session
   - RDP continuously sends screen updates, explaining the consistent data stream
  
---

## Skills & Experience Gained

- **Cloud VM Deployment**: Created and configured Windows 10 and Ubuntu Linux virtual machines within Azure
- **Virtual Networking**: Connected multiple VMs to a shared Virtual Network and Subnet, verifying network topology with Azure Network Watcher
- **Traffic Analysis with Wireshark**: Captured and filtered live network traffic to observe protocol-specific behavior including ICMP, SSH, DHCP, DNS, and RDP
- **Firewall and NSG Management**: Practiced controlling traffic flow by modifying Network Security Group (NSG) rules to block or allow specific protocol traffic
- **Remote Access & Protocol Use**: Used RDP to manage the Windows VM and SSH to securely access the Ubuntu VM
- **DNS & DHCP Functionality**: Interacted with domain name resolution and IP address assignment processes to observe their live network behavior
- **Real-Time Troubleshooting**: Identified and diagnosed connectivity and firewall-related issues through packet inspection and system responses
- **Secure Shell Operations**: Performed secure remote commands to Linux systems while monitoring encrypted data exchange
- **Command Line Proficiency**: Used PowerShell and terminal commands for ping tests, IP renewal, DNS queries, and SSH sessions

## Screenshots

All screenshots for this lab can be found in the [screenshots](./screenshots) folder.
