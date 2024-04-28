# Security Operations Center Automation Lab

## About this project
This project is an open sourced security operation center automation lab that tries to replicate and gain invaluable experience from what a SOC Analyst would do on a daily basis. It builds the infrastructure of a security operations center, as well providing real-time threats and alerts for a SOC Analyst to triage and remediate. The goal here is to automate as much of the process of what a SOC Analyst would do. We will also be implementing automated scripts for incident response, accelerate threat detection and streamline SOC workflows. I expect there will be a lot of errors and breaks during this project which I am all for. 

<br/>

## Tools Used
- Wazuh for EDR (located in the cloud)
- The Hive for case management (located in the cloud)
- Square X for disposable files and browsers
- Shuffle for Security, Orchestration, Automation and Response (SOAR)
- 1 PC or Virtual Machine
- 2 Servers
  
<br/>

## Designing the Lab

The most important part of this Automated SOC Home Lab is the design and diagramming of this project.
We have to map this project logically. Please see diagram below.

<br/>
<a href="https://ibb.co/h1JTdyP"><img src="https://i.ibb.co/vv6MdH7/1v1.png" alt="1v1" border="0"></a>

<br/>
So what is the logic of this system?

Let's break it down into simple steps.
There are a total of 9 steps.

1 - From a Windows 10 Client (Wazuh Agent) send events to the Wazuh Manager.
2 -  A Wazuh Manager will receive these events from the Internet.
3 -  Alerts will be sent from the Wazuh Manager to Shuffle.
4 - Shuffle will send these alerts to the Internet to enrich the Indicators of Compromise (IOC) and receive it back.
5 - Shuffle will then send these IOCs based alerts to TheHive for case management.
6 - Shuffle will also be notified of these alerts sent to TheHive and will send an email to notifiy an SOC analyst.
7 - The email will be sent and received by the SOC Analyst.
8 - The SOC Analyst will send back a response action which will go back to Shuffle and then to the Wazuh Manager.
9 - Wazuh Manager will perform a response action to the Windows 10 Client (Wazuh Agent).

<br/>
Below is a diagram of the purpose of each machine. 

<br/>
<a href="https://ibb.co/Hx5Z7DS"><img src="https://i.ibb.co/Gtzm7v6/2v2.png" alt="2v2" border="0"></a>

<br/>

## Installing Applications & Virtual Machines

We will be installing the following:
1 Windows 10 VM with Sysmon installed
1 Wazuh Server
1 TheHive Server

Wazuh is an open sourced cyber security application that integrates SIEM and XDR capabilities. 

Both Wazuh and TheHive will be using Ubuntu.

First install a Windows VM through Virtual Box.
Here are the specs for the Windows VM:
- [X] 8 gigabytes of memory
- [X] 70 gibabytes of HDD
- [X] Skipped unattended install

Go through the installation process.

```
Select > I don't have a product key
Accept Windows 10 Pro.
Select > Custom Install
```

The installation might take a while. 
Now you have to install "Guest Additions"

In Virtual Box:
```
Click > Devices > Insert Guest Additions CD Image
```
<br/>
Go to This PC > Click on the VirtualBox Guest Additions > Click on the appropriate Guest Addition.

<a href="https://ibb.co/Hx5Z7DS"><img src="https://i.ibb.co/Gtzm7v6/2v2.png" alt="3" border="0"></a>

** Troubleshooting**
Had some errors installing the appropriate Guest Additions.

Solution
I had to upgrade the Guest Additions throught VirtualBox > Device > Upgrade Guest Additions.


Once Windows 10 client is installed, we need to install Windows Sysmon.


Sysmon is Windows system monitor. It monitors and logs system activity to the
Windows event log. It is part of the Sysinternals suite of tools 
provided by Microsoft. Sysmon is designed to help with the detection of advanced 
security threats and to provide data that can be crucial for effective forensic analysis of a system.

Sysmon is great for getting more detailed view of what is happening to the system,
including signs of malicious activity.

Time to install Sysmon.

This is a bit trickier than I thought.

Go to Microsoft's Sysmon download url:
```
https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon#introduction
```

```
Click > Download Sysmon
```




