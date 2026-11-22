# SOC Automation SIEM Detection

## Objective

Build a detection lab SOC analyst pipeline on Microsoft Azure, for simulating and detecting cyber attacks. Primary focus was to ingest, make a system with defined rules, and automate a structured system that flow easly to send alerts of important, high priority, and monitored logs from Security Information and Event Management (SIEM), to each analyst, using Security Orchestration, Automation, and Response (SOAR). This hand-on experience was to deepen understanding of SIEM rules, the flow using Security Orchestration, Automation, and Response (SOAR), while maintaining tight secure environment between the cloud.

### Skills Learned

- Deploying cloud virtual machines on Azure. With controlled access with Network Security Groups.
- Advanced understanding of SIEM concepts and practical application. By (in this case) installing and configuring a SIEM (Wazuh) and enrolling a Windows agent in virtual machine.
- Collecting endpoint telemetry with Sysmon.
- Writing custom Wazuh detection rules (XML and regex) and mapping them according to MITRE ATT&CK.
- Building SOAR workflows in Shuffle (webhooks, API calls, JSON handling)
- Setting up TheHive with Cassandra and Elasticsearch, and using service accounts with API keys.
- Enriching alerts with the VirusTotal.
- Linux administration: systemd services, file permissions and log analysis.
- Troubleshooting step by step: service, port, network, authentication, data.

### Tools Used

- Microsoft Azure,	Two private virtual machines, one for Wazuh and one for TheHive
- Wazuh	SIEM: log collection, detection and alerting
- Sysmon Windows process events
- Windows 11 (virtual machine)	Monitored endpoint, with the Wazuh agent installed.
- Shuffle	SOAR, For automation workflow.
- TheHive [6.0.0]	Case and alert management
- Cassandra [4.1.12]	TheHive database
- Elasticsearch [8.19.25]	TheHive search index

## Steps
With these diagram as follow, these are the steps that went down.
![Alt text description](./FlowDiagram.jpg)
*Ref 1: Automation Diagram*

1. Deploy the Wazuh server (Azure VM 1)
Created an Azure VM, installed Wazuh, and restricted the dashboard and SSH ports to my own IP in the Network Security Group.

*Ref 2: Network Diagram*
[SCREENSHOT: Wazuh dashboard, with IPs blurred]

2. Connect the Windows 11 endpoint
Installed the Wazuh agent and Sysmon on a Windows 11 virtual machine, opened ports 1514 and 1515 for the agent's IP only, and confirmed that the agent showed as Active.

*Ref 3: Network Diagram*
[SCREENSHOT: agent status in Endpoints summary]

3. Deploy TheHive (Azure VM 2)
Installed Cassandra, Elasticsearch and TheHive on a second VM. Elasticsearch runs in single-node mode and, like Cassandra, listens on 127.0.0.1 only. Only TheHive's port is reachable, and only from my IP.

*Ref 4: Network Diagram*
[SCREENSHOT: TheHive login page or case list]

4. Write the detection rule
Created a custom Wazuh rule that triggers on a Sysmon process-creation event using the original file name (in this case) is Mimikatz.

5. Build the Shuffle workflow
- Webhook receives the Wazuh alert
- The SHA256 value is extracted from the Sysmon hash field
- VirusTotal returns the reputation of the hash
- A TheHive alert is created through a service account (API key)
- An email notification is sent

![Alt text description](./SOAR_Shuffle.png)
*Ref 5: Shuffle Workflow*

6. Test the whole chain
Ran a test in my own isolated virtual machine and followed the alert through each tool. Result:

*Ref 6: Email & Notification recieved*
And it run as intended. it can be configured futher according to individual needs or to prevent an attack surge across the globe.
__________________________________________________________________
And lastly, on that note: why I did not include *Suricata* (IDS) in this project: This lab focuses on endpoint detection (Wazuh and Sysmon). Suricata would add network-level detection (IDS/IPS) and give extra visibility, for port scans and known malicious traffic. But in this case Suricata would produces a large number of low-priority alerts, in which it would bury the Mimikatz detection for this project. Because the base point of this project to just see wazuh works and the endpoint is monitored but if the workflow, would flow flawlessly, until it was recieved in email, and analyst hands.
