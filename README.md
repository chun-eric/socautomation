# Security Operations Center Automation Lab

## About this project

## Tools Used
- Wazuh for EDR (located in the cloud)
- The Hive for case management (located in the cloud)
- Square X for disposable files and browsers
- Shuffle for Security, Orchestration, Automation and Response (SOAR)
- 1 PC or Virtual Machine
- 2 Servers
  

## Designing the Lab

The most important part of this Automated SOC Home Lab is the design and diagramming of this project.
We have to map this project logically. Please see diagram below.

<br/>
<a href="https://ibb.co/h1JTdyP"><img src="https://i.ibb.co/vv6MdH7/1v1.png" alt="1v1" border="0"></a>

<br/>
So what is the logic of this system?

Let's break it down into simple steps.
There are a total of 9 steps.

- [1] From a Windows 10 Client (Wazuh Agent) send events to the Internet
- [2] A Wazuh Manager will receive these events from the Internet
- [3] Alerts will be sent from the Wazuh Manager to Shuffle
- [4] Shuffle will send these alerts to the Internet to enrich the Indicators of Compromise (IOC) and receive it back
- [5] Shuffle will then send these IOCs based alerts to TheHive for case management
- [6] Shuffle will also be notified of these alerts sent to TheHive and will send an email to notifiy an SOC analyst 
- [7] The email will be sent and received by the SOC Analyst
- [8] The SOC Analyst will send back a response action which will go back to Shuffle and then to the Wazuh Manager
- [9] Wazuh Manager will perform a response action to the Windows 10 Client (Wazuh Agent)


<a href="https://ibb.co/Hx5Z7DS"><img src="https://i.ibb.co/Gtzm7v6/2v2.png" alt="2v2" border="0"></a>
