

 ### [YouTube Demonstration](https://youtu.be/7eJexJVCqJo)

![col-pr-msft-azure-sentinel](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/9ab55ce6-0954-4892-a1c6-c3b0bfbb45e8)

<h1>SIEM Implementation in Azure</h1>

<h2>Summary</h2>

SIEM, or Security Information and Event Management, aids organizations in spotting, analyzing, and tackling security threats proactively. It gathers event log data from various network sources like Firewalls, IDS/IPS, and Identity solutions. This enables security experts to track, prioritize, and address potential threats in real-time. A honeypot serves as a security mechanism, setting a virtual trap in a controlled environment to attract attackers. It deliberately compromises a computer system to study attackers' methods, explore different threat types, and enhance security measures. This lab aims to grasp the process of gathering honeypot attack log data, querying it within a SIEM, and presenting it in an easily comprehensible manner. Specifically, it will be showcased on a world map based on event count and geolocation.

<br />

<h2>Learning Objectives</h2>

- Configuration & Deployment of Azure resources such as virtual machines, Log Analytics Workspaces, and Azure Sentinel
- Hands-on experience and working knowledge of a SIEM Log Management Tool (Microsoft's Azure Sentinel)
- Understand Windows Security Event logs
- Utilization of KQL to query logs
- Display attack data on a dashboard with Workbooks (World Map)

<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Kusto Query Language (KQL)</b>
- <b>Azure Security Center</b>
- <b>Azure Sentinel</b>
- <b>3rd Party API: http://ipgeolocation.io</b>
- <b>Remote Desktop Protocol (RDP)</b>

<h2>Environments Used </h2>

- <b>Windows 11</b> (21H2)


<h2>Overview</h2>

 ![SIEM Lab](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/086407fa-6c4e-42a2-823b-8a7d2202cf01)

<h2>Step 1: Create a Microsoft Azure Subscription</h2>

![subscription](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/d95487ae-30fd-47d2-bcbb-a04635d4e543)


<h2>Step 2: Create a Honeypot Virtual Machine</h2>

![create_azure_vm](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/f46f04f3-3921-4fb6-a6ba-6a2541cf0190)


<h2>Basics</h2>

- Go to portal.azure.com
- Search for "virtual machines"
- Create > Azure virtual machine


<h3>Project Details</h3>

- Create new resource group and name it (honeypotlab)


<h3>Instance details</h3>

- Name your VM (honeypot-vm)
- Select a recommended region ((US) West US 2)
- Availability options: No infrastructure redundancy required
- Security type: Standard
- Image: Windows 11 Pro, version 22H2 
- VM Architecture: x64
- Size: Default is okay Standard D2s v3 (2 vcpus, 8 GiB memory)

<h3>Administrator account</h3>

- Create a username and password for virtual machine

<h3>Inbound port rules</h3>

- Public inbound ports: Allow RDP (3389)


<h3>Licensing</h3>

- Confirm licensing
- Select Next : Disks >
![vm1](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/2a068667-8887-44e3-bcea-1206bda4cb56)

<h3>Disks</h3>

- Leave all defaults
- Select Next : Networking >


<h3>Networking</h3>

<h4>Network interface</h4>

- NIC network security group: Advanced > Create new
- Remove Inbound rules (1000: default-allow-rdp) by clicking three dots
- Destination port ranges: * (wildcard for anything)
- Protocol: Any
- Action: Allow
- Priority: 100 (low)
- Name: Anything (ALLOW_ALL_INBOUND)
- Select Review + create

![network_sec_grp](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/9fadd1e9-7c39-4f77-aa80-ff2621c3286f)

<h1>Step 3: Create a Log Analytics Workspace</h1>

- Search for "Log analytics workspaces"
- Select Create Log Analytics workspace
- Put it in the same resource group as VM (honeypotlab)
- Give it a desired name (honeypot-log)
- Add to same region (West US 2)
- Select Review + create

![log_an_wrk](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/513ffa30-9467-46d8-b632-a5de3a121859)



<h1>Step 4: Configure Microsoft Defender for Cloud</h1>

- Search for "Microsoft Defender for Cloud"
- Scroll down to "Environment settings" > subscription name > log analytics workspace name (log-honeypot)

![mcrsft_dfndr](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/ca7fbd05-8f5d-4d9d-8550-b3f39a86bc89)


<h3>Settings | Defender plans</h3>

- Cloud Security Posture Management: ON
- Servers: ON
- SQL servers on machines: OFF
- Hit Save

![defender_plans](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/b0b320ed-aa3b-4bbd-8b44-ba2602beaa26)

<h3>Settings | Data collection</h3>

- Select "All Events"
- Hit Save


<h1>Step 5: Connect Log Analytics Workspace to Virtual Machine</h1>

- Search for "Log Analytics workspaces"
- Select workspace name (log-honeypot) > "Virtual machines" > virtual machine name (honeypot-vm)
- Click Connect

![log_an_vm_connect](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/71b8ba57-6652-4a83-a5ee-f0251fed339f)


<h1>Step 4: Step 6: Configure Microsoft Sentinel</h1>

- Search for "Microsoft Sentinel"
- Click Create Microsoft Sentinel
- Select Log Analytics workspace name (honeypot-log)
- Click Add

![sentinel_log](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/f50bd083-a480-440f-89c2-a50c2e01cbb1)








<br />
<br />


<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
