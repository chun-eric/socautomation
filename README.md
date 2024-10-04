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

Wazuh is an open source cyber security application that integrates SIEM and XDR capabilities. 
The main components of Wazuh is the indexer, server and dashboard.

TheHive is an open source security incident response platform. TheHive will be our case management system.

Both Wazuh and TheHive will be using Ubuntu.

We will use a Windows 10 VM with Ubuntu installed as our client machine. This Windows VM will also have Sysmon installed. 

What is Sysmon? Sysmon is short for System Monitor and is developed by Microsoft. 
In a nutshell, Sysmon it helps track and log important activities on a Windows computer, 
especially processes, network connections, and changes to files or registry keys. 


### First install a Windows VM through Virtual Box.
Here are the specs for the Windows VM:
- [X] 8 gigabytes of memory
- [X] 70 gibabytes of HDD
- [X] Skipped unattended install

Go through the installation process.
Verify the SHA256 checksum with powershell.
In a powershell window type:

```
Get-FileHash ./{name of the virtualbox.exe}
```

Your Hash is generated.
Copy and paste it in the virtualbox SHA256 hashes to ensure it is there. 

If it is there that means there was no change during transit. 

Now that the Virtual Box program is safe to use, install VirtualBox. 
Download any dependencies and missing redistributables to ensure VirtualBox compatibility with Windows 10 VM.

Go through the installation process to finish installing Virtual Box. 

Once Virtual Box is installed go to:

```
Windows 10 installation media link > Download Tool Now
```

This will create a Windows ISO image file. 
Make sure to choose > ```Create installation medai for another PC```

choose the: ```ISO file```

```
Select > I don't have a product key
Accept Windows 10 Pro.
Select > Custom Install
```

### Creating a Windows VM in VirtualBox
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

![image](https://github.com/user-attachments/assets/7f7857a9-79d5-490c-9f75-78ef2803a870)


This is a bit trickier than I thought.

Go to Microsoft's Sysmon download url:
```
https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon#introduction
```

```
Click > Download Sysmon
```

This will be in a zipped file. Extract all. 

We need to go to a Github repo called sysmon-moular and download the file
called sysmonconfig.

```
https://github.com/olafhartong/sysmon-modular/blob/master/sysmonconfig.xml
```

<a href="https://ibb.co/Hx5Z7DS"><img src="https://i.ibb.co/Gtzm7v6/2v2.png" alt="3" border="0"></a>

![image](https://github.com/user-attachments/assets/147a40c3-e07b-41d5-8718-9a348bf0b79b)

```
Save the file as Right-click > Save As > Name it sysmonconfig
```


Open the file as Administrator through Powershell.

![image](https://github.com/user-attachments/assets/c0a1e30e-bda0-4157-83ca-0f62176db078)

In Powershell you have to be in the same directory as the extracted files. 


In Powershell type:
```
cd "[copy file path]"
```

Copy the sysmonconfig.xml into the extracted files folder. It should look like this.

![image](https://github.com/user-attachments/assets/491db250-b118-4c44-b6c2-2fdecb075f2a)


Now that you are in the correct directory in the Sysmon folder with the sysmonconfig file also inside,
look at the contents of the folder
by typing:
```
dir
```

![image](https://github.com/user-attachments/assets/57ab5019-2350-4bfc-9b6a-4825197ad38e)

We need to check that sysmon is installed properly. 

Go to: ```Services > type Sysmon```

![image](https://github.com/user-attachments/assets/da1808e0-9bc6-4967-9255-52ad0ec849f1)

Alternatively you can go to: ```Event Viewer > Application and Services Logs > Microsoft > Windows```

![image](https://github.com/user-attachments/assets/a1964616-6fc3-42eb-92df-27021c0d1d27)


Currently there is no Sysmon installed.


Now we need to make sure Sysmon is installed. 

In Powershell type in the following command:
```
.\sysmon64.exe -i sysmonconfig.xml
```

The above command means install sysmon with the configuration file. 
The configuration file will log events based on rules such process creation, network connections, file changes etc..

![image](https://github.com/user-attachments/assets/a686eaec-c667-498e-a5c3-5bc3401a915b)


Double check that sysmon is installed. 

![image](https://github.com/user-attachments/assets/c97db395-e4fd-4153-866e-3fb7eb502516)

![image](https://github.com/user-attachments/assets/4726733b-9fa3-40f1-b062-802855eae213)


Now that Sysmon is installed. We can start setting up Wazuh next. 

### Installing Wazuh 










