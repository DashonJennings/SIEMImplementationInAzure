

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


<h1>Step 6: Configure Microsoft Sentinel</h1>

- Search for "Microsoft Sentinel"
- Click Create Microsoft Sentinel
- Select Log Analytics workspace name (honeypot-log)
- Click Add

![sentinel_log](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/f50bd083-a480-440f-89c2-a50c2e01cbb1)



<h1>Step 7: Disable the Firewall in Virtual Machine</h1>

- Go to Virtual Machines and find the honeypot VM (honeypot-vm)
- By clicking on the VM copy the IP address
- Log into the VM via Remote Desktop Protocol (RDP) with credentials from step 2
- Accept Certificate warning
- Select NO for all Choose privacy settings for your device
- Click Start and search for "wf.msc" (Windows Defender Firewall)
- Click "Windows Defender Firewall Properties"
- Turn Firewall State OFF for Domain Profile Private Profile and Public Profile
- Hit Apply and Ok
- Ping VM via Host's command line to make sure it is reachable ping -t <VM IP>

![defender_off](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/218389ab-8d7b-483c-8c10-3d048ca89f93)


<h1>Step 8: Scripting the Security Log Exporter</h1>

- In VM open Powershell ISE
- Set up Edge without signing in
- Copy Powershell script into VM's Powershell (Written by Josh Madakor)
- Select New Script in Powershell ISE and paste script
- Save to Desktop and give it a name (Log_Exporter)

![powershell_script](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/bcf9b1c4-4a16-4996-9f04-ec1af1515ad8)


- Make an account with Free IP Geolocation API and Accurate IP Lookup Database
- Copy API key once logged in and paste into script line 2: $API_KEY = "<API key>"
- Hit Save
- Run the PowerShell ISE script (Green play button) in the virtual machine to continuously produce log data

![ipgeolocation](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/12750f92-2323-4862-a7e2-f3100cb5e302)



<h1>Step 9: Create Custom Log in Log Analytics Workspace</h1>

- Create a custom log to import the additional data from the IP Geolocation service into Azure Sentinel
- Search "Run" in VM and type "C:\ProgramData"
- Open file named "failed_rdp" hit CTRL + A to select all and CTRL + C to copy selection
- Open notepad on Host PC and paste contents
- Save to desktop as "failed_rdp.log"
- In Azure go to Log Analytics Workspaces > Log Analytics workspace name (honeypot-log) > Custom logs > Add custom log

<h3>Sample</h3>

- Select Sample log saved to Desktop (failed_rdp.log) and hit Next


<h3>Record delimiter</h3>

- Review sample logs in Record delimiter and hit Next


<h3>Collection paths</h3>

- Type > Windows
- Path > "C:\ProgramData\failed_rdp.log"


<h3>Details</h3>

- Give the custom log a name and provide description (FAILED_RDP_WITH_GEO) and hit Next
- Hit Create

![custom_log](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/62b2ffa9-aa33-4284-9a3e-0bb023be6f3a)


<h1>Step 10: Query the Custom Log</h1>

- In Log Analytics Workspaces go to the created workspace (honeypot-log) > Logs
- Run a query to see the available data (FAILED_RDP_WITH_GEO_CL)

![failed_rdp_with_geo](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/50cce716-84d8-4f32-b4ef-49b442ec6432)


<h1>Step 11: Extract Fields from Custom Log</h1>

- Right click any of the log results
- Select Extract fields from 'FAILED_RDP_WITH_GEO_CL'
- Highlight ONLY the value after the ":"
- Name the Field Title the name of the field of the value
- Under Field Type select the appropriate data type
- Hit Extract
- If the search results data looks good click the Save extraction button
- Do this for ALL available fields in RawData

![data_extraction](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/9a4f6b45-77da-4661-8dba-4a6292102525)


<h1>Step 12: Map Data in Microsoft Sentinel</h1>

- Go to Microsoft Sentinel to see the Overview page and available events
- Click on Workbooks and Add workbook then click Edit
- Remove default widgets (Three dots > Remove)
- Click Add > Add query
- Copy/Paste the following query into the query window and Run Query

FAILED_RDP_WITH_GEO_CL | summarize event_count=count() by sourcehost_CF, latitude_CF, longitude_CF, country_CF, label_CF, destinationhost_CF
| where destinationhost_CF != "samplehost"
| where sourcehost_CF != ""

- Once results come up click the Visualization dropdown menu and select Map
- Select Map Settings for additional configuration

<h3>Layout Settings</h3>

- Location info using > Latitude/Longitude
- Latitude > latitude_CF
- Longitude > longitude_CF
- Size by > event_count


<h3>Color Settings</h3>

- Coloring Type: Heatmap
- Color by > event_count
- Aggregation for color > Sum of values
- Color palette > Green to Red

<h3>Metric Settings</h3>

- Metric Label > label_CF
- Metric Value > event_count
- Select Apply button and Save and Close
- Save as "Failed RDP World Map" in the same region and under the resource group (honeypotlab)
- Continue to refresh map to display additional incoming failed RDP attacks


![failed_rdp_map](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/49192215-bf12-4af9-a520-0e65e0dd8df4)

![event_viewer](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/e27cb4f1-0e75-4254-8ad1-00aae70ab8ee)

![rdp_script](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/1f5c5bad-744e-4888-8ff6-c125dcf5f4ac)



<h1>Step 13: Deprovision Resources</h1>

- Search for "Resource groups" > name of resource group (honeypotlab) > Delete resource group
- Type the name of the resource group ("honeypotlab") to confirm deletion
- Check the Apply force delete for selected Virtual machines and Virtual machine scale sets box
- Select Delete

![delete_resource_group](https://github.com/DashonJennings/SIEMImplementationInAzure/assets/160358839/5daf76fa-0984-41f6-b092-2ded7bddcd42)







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
