

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


<h1>Project Details</h1>








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
