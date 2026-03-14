# Project 7: Active Response Threat Mitigation

## Project Overview
In this final project, I transitioned from Passive Monitoring to Active Defense. I configured the Wazuh SIEM to automatically respond to a high-severity Brute Force attack by dynamically updating the host's firewall to block the attacker’s IP address.

## The Scenario
Attacker: Kali Linux (192.168.0.115) performing an SSH Brute Force attack.
Defender: Ubuntu Manager/SOC Analyst.
Goal: Detect the attack in real-time and "drop the gate" using the local firewall to prevent further access.

## What Happened? (The Brief Summary)
### Detection: 
Wazuh triggered Rule 2502 (Level 10) after 8+ failed SSH login attempts from the Kali machine.
### The Challenge: 
During automation, the Active Response script encountered a "field mapping error." The script looked for a srcip field, but the SSH logs provided the IP in the rhost field.
### The Solution: 
I performed manual Mitigation Injection. Using the Linux kernel's firewall (iptables), I applied a DROP rule for the specific attacker IP to simulate the successful completion of the Active Response loop.
### Verification: 
I confirmed that the attacker could no longer communicate with the server (Ping Timeout).

## Technical Implementation
Configuration: Modified ossec.conf to define the <active-response> block and the <command> to execute firewall-drop.
Mitigation Command:
sudo iptables -I INPUT -s 192.168.0.115 -j DROP
Verification Command:
sudo iptables -L -n | grep 192.168.0.115

## Key Takeaways for My Portfolio
Automation Logic: Learned how SIEM tools bridge the gap between "Alerting" and "Action."
Troubleshooting: Successfully diagnosed script execution failures by analyzing active-responses.log.
Defense-in-Depth: Demonstrated that even when automation fails, a SOC Analyst must be able to perform manual mitigation to protect the infrastructure.
