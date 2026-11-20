# SOC Automation SIEM Detection

## Objective

Build a detection lab SOC analyst pipeline on Microsoft Azure, for simulating and detecting cyber attacks. Primary focus was to ingest, make a system with defined rules, and automate a structured system that flow easly to send alerts of important, monitored logs from Security Information and Event Management (SIEM), to each analyst, using Security Orchestration, Automation, and Response (SOAR). This hand-on experience was design to deepen undrstanding fo

The Detection Lab project aimed to establish a controlled environment for simulating and detecting cyber attacks. The primary focus was to ingest and analyze logs within a Security Information and Event Management (SIEM) system, generating test telemetry to mimic real-world attack scenarios. This hands-on experience was designed to deepen understanding of network security, attack patterns, and defensive strategies.

### Skills Learned
[Bullet Points - Remove this afterwards]

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used
[Bullet Points - Remove this afterwards]

- Microsoft Azure	Two private virtual machines, one for Wazuh and one for TheHive
- Wazuh	SIEM: log collection, detection and alerting
- Sysmon	Detailed Windows process events
- Windows 11 (virtual machine)	Monitored endpoint, with the Wazuh agent installed
- Shuffle	SOAR: automation workflow
- TheHive [VERSION]	Case and alert management
- Cassandra [VERSION]	TheHive database
- Elasticsearch [VERSION]	TheHive search index
- VirusTotal API

- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Steps
drag & drop screenshots here or use imgur and reference them using imgsrc

Every screenshot should have some text explaining what the screenshot is about.

Example below.

*Ref 1: Network Diagram*

__________________________________________________________________
Why I did not include Suricata, in this project: This lab focuses on endpoint detection (Wazuh and Sysmon). Suricata would add network-level detection (IDS/IPS) and give extra visibility, for port scans and known malicious traffic. Suricata produces a large number of low-priority alerts, in which it would bury the Mimikatz detection for this project.
