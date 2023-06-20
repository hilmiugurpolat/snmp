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

to be continued
