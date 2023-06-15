# SNMP
Simple Network Management Protocol(SNMP)

SNMP is an application layer protocol for the exchange of administrative information across network devices.

To better understand the snmp protocol, I will install a virtual machine on my computer (ubuntu server). From now on i will call my virtual machine as agent computer and my local as admin computer

How do I enable SNMP on Ubuntu?
run the following commands in the terminal on the agent computer:

1.Update all packages: **sudo apt-get update**

2.Install SNMP:**sudo apt-get install snmpd**

3.Edit the snmpd.conf file with any text editor. Type: **sudo nano /etc/snmp/snmpd.conf**

4.Configure agentAddress: **"agentAddress udp:161"**

5.Configure rocommunity name like: **rocommunity public**

-Change “public” to your community name

6.Restart the SNMPD service: **sudo service snmpd restart**

7.Check that SNMPD is started OK with this: **sudo service snmpd status**
