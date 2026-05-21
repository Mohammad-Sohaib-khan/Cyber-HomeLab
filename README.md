🛡️ Azure SOC Homelab (Microsoft Sentinel SIEM Project)
📌 Overview

This project demonstrates the setup of a basic Security Operations Center (SOC) in Microsoft Azure using a free-tier subscription. The lab simulates real-world attack scenarios using a deliberately exposed virtual machine (honeypot) and centralizes log analysis through Microsoft Sentinel (SIEM).

The goal of this project is to gain hands-on experience in:
Cloud security monitoring
Threat detection and analysis
SIEM configuration
Log ingestion and visualization

🎯 Objectives
Deploy a vulnerable Windows/Linux virtual machine in Azure
Configure it as a honeypot exposed to the internet
Collect and forward logs to a centralized Log Analytics Workspace
Integrate Microsoft Sentinel for threat detection
Analyze failed login attempts and attacker behavior
Visualize global attack sources using geolocation mapping

🏗️ Architecture
The lab environment consists of:

Azure Virtual Machine (Honeypot)
Network Security Group (NSG) allowing inbound traffic
Log Analytics Workspace
Microsoft Sentinel (SIEM)
Azure Monitor / Diagnostic Settings

⚙️ Setup Process
1. Azure Subscription & Virtual Machine

A free Azure subscription was used to deploy a virtual machine configured as a public-facing system to simulate a realistic attack surface.

2. Log Analytics Workspace

A Log Analytics Workspace was created to centralize and store logs generated from the virtual machine.

3. Log Forwarding

Diagnostic settings were configured to forward:

Security logs
Authentication logs
System events

to the Log Analytics Workspace.

4. Microsoft Sentinel Integration

Microsoft Sentinel was connected to the workspace to enable:

Threat detection
Log querying using KQL
Security event correlation

📊 Key Analyses Performed
Using Kusto Query Language (KQL), the following were analyzed:

Failed login attempts (brute force detection)
IP address tracking of attackers
Geographic distribution of attack sources
Frequency of malicious login attempts

🔍 KQL Query Examples (Threat Detection)

This section includes sample KQL queries used in Microsoft Sentinel to analyze attacker behavior, failed logins, and suspicious activity from the honeypot VM.

🚨 1. Failed Login Attempts (Brute Force Detection)
This query identifies repeated failed login attempts, which may indicate brute-force attacks.

SecurityEvent

| where EventID == 4625

| summarize FailedAttempts = count() by IpAddress, Account, bin(TimeGenerated, 5m)

| order by FailedAttempts desc

🌍 2. Top Attacker IP Addresses
This query shows the most active malicious IPs targeting the system.

SecurityEvent

| where EventID == 4625

| summarize AttackCount = count() by IpAddress

| top 10 by AttackCount desc

📍 3. Geographic Distribution of Attacks
If geo-IP enrichment is enabled in Sentinel, this shows where attacks originate globally.

SecurityEvent

| where EventID == 4625

| extend Geo = tostring(IpAddress)

| summarize Count = count() by Geo

| order by Count desc

👤 4. Failed Login Attempts by Username
Useful for identifying targeted account attacks.

SecurityEvent

| where EventID == 4625

| summarize Attempts = count() by Account

| order by Attempts desc

⏱️ 5. Time-Based Attack Pattern Analysis
This shows when attackers are most active.

SecurityEvent

| where EventID == 4625

| summarize AttackVolume = count() by bin(TimeGenerated, 1h)

| render timechart

⚠️ 6. Suspicious Login Pattern (Multiple IPs per Account)
Detects accounts being attacked from multiple IPs (possible credential stuffing).

SecurityEvent

| where EventID == 4625

| summarize IPCount = dcount(IpAddress) by Account

| where IPCount > 3

| order by IPCount desc

🌍 Attack Map Visualization
Microsoft Sentinel’s map visualization tool was used to plot attacker IP addresses globally, showing real-time cyber attack origins targeting the honeypot VM.

🧪 Example Use Cases
Brute force SSH/RDP detection
Geographic threat intelligence analysis
SOC alert simulation and response practice
SIEM rule testing and tuning

🧰 Tools & Technologies
Microsoft Azure
Azure Virtual Machines
Azure Monitor
Log Analytics Workspace
Microsoft Sentinel (SIEM)
Kusto Query Language (KQL)

📈 Key Skills Demonstrated
Cloud security architecture
SIEM deployment and configuration
Log analysis and threat detection
Incident monitoring and visualization
Cybersecurity operations (SOC fundamentals)

🚀 Outcome
This project provides practical exposure to real-world SOC operations, simulating how security analysts detect and respond to cyber threats in cloud environments.

It demonstrates foundational skills in cloud security engineering and SOC analysis, making it valuable for entry-level cybersecurity roles.

👤 Author

Mohammad Sohaib Khan

Cybersecurity & Cloud Security Enthusiast
