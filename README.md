# SNMP
Simple Network Management Protocol(SNMP)

SNMP (Simple Network Management Protocol) is a network management protocol that provides communication between network devices and network management systems. SNMP is used to monitor the status of network devices, detect errors, evaluate performance, and perform management operations.

To better understand the snmp protocol, I will install a virtual machine on my computer (ubuntu server). From now on i will call my virtual machine as agent computer and my local as admin computer

How do I enable SNMP on Ubuntu?
run the following commands in the terminal on the agent computer:

1.Update all packages: **`sudo apt-get update`**

2.Install SNMP: **`sudo apt-get install snmpd`**

3.Edit the snmpd.conf file with any text editor. Type: **`sudo nano /etc/snmp/snmpd.conf`**

4.Configure agentAddress: **`agentAddress udp:161`**

5.Configure rocommunity name like: **`rocommunity public`**

-Change “public” to your community name

6.Restart the SNMPD service: **`sudo service snmpd restart`**


7.Check that SNMPD is started OK with this: **`sudo service snmpd status`**
![IMG_20230620_090201](https://github.com/hilmiugurpolat/snmp/assets/110428681/5e11c967-0f90-4c97-88de-efd0eea558b0)
if the output is like this, snmp service is running correctly


# MIB
MIB is a structure used in network management protocols. A MIB is a collection of data items that describe the state, configuration, and other properties of managed devices. When communicating with SNMP (Simple Network Management Protocol), admin clients use MIB files to get management information from SNMP agents (managed devices).

You can follow the steps below to enable MIBs in Linux operating system:
1. Downloading MIB files: **`sudo apt-get install snmp-mibs-downloader`**
2. Defining MIB files: **`sudo nano /etc/snmp/snmp.conf`**
3. Add this line inside the file: **`mibs +ALL`** and **`view all included .1 80 mibs +ALL`**
4. You can use the following command to check active MIB files: **`snmpwalk -v2c -c public localhost`**
   
![Screenshot from 2023-06-20 12-44-06](https://github.com/hilmiugurpolat/snmp/assets/110428681/946e5b8d-7bcf-46e0-aab3-838c0cb74ebf)
 If this output is output, then the operation is correct.


# CUSTOM MIB
Creating a customized MIB file is used to define management information for a specific need outside of standard MIB files.

For example, we can create a custom MIB file for the NTP service. This file may contain administrative information of NTP servers, statistics about time synchronization, and configurations. In this way, we can access and manage NTP service related information using this custom MIB file over SNMP protocol.

You can follow the steps below to download ntp service and create custom mib file

1.Downloading NTP:

-You can use this command to download NTP: **`sudo apt-get install ntp`**

2.Creating the custom MIB file:

-To create the custom MIB file, first go to **`/usr/share/snmp/mibs`**

-**`cd /usr/share/snmp/mibs`*

3.Then you can use a text editor to create the custom MIB file. For example, create a new file with the Nano text editor using the following command:
-**`sudo nano ntp.txt`**

to be continued .. 







