# SNMP
Simple Network Management Protocol(SNMP)

SNMP (Simple Network Management Protocol) is a network management protocol that provides communication between network devices and network management systems. SNMP is used to monitor the status of network devices, detect errors, evaluate performance, and perform management operations.

To better understand the snmp protocol, I will install a virtual machine on my computer (ubuntu server). From now on i will call my virtual machine as agent computer and my local as admin computer

How do I enable SNMP on Ubuntu?
run the following commands in the terminal on the agent computer:

1.Update all packages:  `sudo apt-get update`

2.Install SNMP:   `sudo apt-get install snmpd`

3.Edit the snmpd.conf file with any text editor.  Type:  `sudo nano /etc/snmp/snmpd.conf`

4.Configure agentAddress: `agentAddress udp:161`

5.Configure rocommunity name like: `rocommunity public`

-Change “public” to your community name

6.Restart the SNMPD service: `sudo service snmpd restart`


7.Check that SNMPD is started OK with this: `sudo service snmpd status`
![IMG_20230620_090201](https://github.com/hilmiugurpolat/snmp/assets/110428681/5e11c967-0f90-4c97-88de-efd0eea558b0)
if the output is like this, snmp service is running correctly


# MIB
MIB is a structure used in network management protocols. A MIB is a collection of data items that describe the state, configuration, and other properties of managed devices. When communicating with SNMP (Simple Network Management Protocol), admin clients use MIB files to get management information from SNMP agents (managed devices).

You can follow the steps below to enable MIBs in Linux operating system:
1. Downloading MIB files: `sudo apt-get install snmp-mibs-downloader`
2. Defining MIB files: `sudo nano /etc/snmp/snmp.conf`
3. Add this line inside the file: `mibs +ALL` and `view all included .1 80 mibs +ALL`
4. You can use the following command to check active MIB files: `snmpwalk -v2c -c public localhost`
   
![Screenshot from 2023-06-20 12-44-06](https://github.com/hilmiugurpolat/snmp/assets/110428681/946e5b8d-7bcf-46e0-aab3-838c0cb74ebf)
 If this output is output, then the operation is correct.


# CUSTOM MIB
Creating a customized MIB file is used to define management information for a specific need outside of standard MIB files.

For example, we can create a custom MIB file for the NTP service. This file may contain administrative information of NTP servers, statistics about time synchronization, and configurations. In this way, we can access and manage NTP service related information using this custom MIB file over SNMP protocol.

You can follow the steps below to download ntp service and create custom mib file

1.Downloading NTP:

-You can use this command to download NTP: sudo apt-get install ntp`

2.Creating the custom MIB file:

-To create the custom MIB file, first go to `/usr/share/snmp/mibs`

-`cd /usr/share/snmp/mibs`

-Then you can use a text editor to create the custom MIB file. For example, create a new file with the Nano text editor using the following command:
-`sudo nano ntp.txt`

3.Generating the Contents of the Custom MIB File:
Let's open the "ntp.txt" file we created and edit its content as follows:
      
```
NTPv4-MIB  DEFINITIONS ::= BEGIN

IMPORTS MODULE-IDENTITY,OBJECT-TYPE,Integer32,Unsigned32,enterprises FROM SNMPv2-SMI  -- RFC2578

MODULE-COMPLIANCE,OBJECT-GROUP FROM SNMPv2-CONF -- RFC2580

 Utf8String
        FROM SYSAPPL-MIB; -- RFC2287

ntpSnmpMIB MODULE-IDENTITY

  LAST-UPDATED "202203270000Z"
       ORGANIZATION "My Organization"
       CONTACT-INFO "Email: "
       DESCRIPTION "Custom MIB for monitoring NTP service"
       REVISION "202203270000Z"
       DESCRIPTION "Initial revision"
       ::= {enterprises 1023 }

ntpSnmpMIBObjects OBJECT IDENTIFIER ::= {ntpSnmpMIB 1 }

ntpEntInfo         OBJECT IDENTIFIER ::= { ntpSnmpMIBObjects 1 }
ntpEntStatus       OBJECT IDENTIFIER ::= { ntpSnmpMIBObjects 2 }

ntpEntSoftwareName OBJECT-TYPE
    SYNTAX      Utf8String
    MAX-ACCESS  read-only
    STATUS      current
    DESCRIPTION
        "The product name of the running NTP version, e.g. 'ntpd'"
    ::= { ntpEntInfo  4 }

ntpEntTimeResolution OBJECT-TYPE
    SYNTAX      Unsigned32
    MAX-ACCESS  read-only
    STATUS      current
    DESCRIPTION
        "The time resolution in integer format, where the resolution
        is represented as divisions of a second, e.g. a value of 1000
        translates to 1.0 ms."
    ::= { ntpEntInfo 2 }

ntpEntTimePrecision OBJECT-TYPE
    SYNTAX      Integer32
    MAX-ACCESS  read-only
    STATUS      current
    DESCRIPTION
        "The entity's precision in integer format, shows the precision.
         A value of -5 would mean 2^-5 = 31.25 ms"
   ::= { ntpEntInfo  3 }

ntpStatus OBJECT-TYPE
  SYNTAX Integer32
  MAX-ACCESS read-only
  STATUS current
  DESCRIPTION "The synchronization status of the NTP server"
  ::= {ntpEntStatus 1 }
ntpEntStatusActiveRefSourceId OBJECT-TYPE
    SYNTAX      Unsigned32 ( 0..99999 )
    MAX-ACCESS  read-only
    STATUS      current
    DESCRIPTION
        "The association ID of the current syspeer."
    ::= { ntpEntStatus 2}



ntpEntConformance OBJECT IDENTIFIER ::= { ntpSnmpMIB 2 }


ntpEntCompliances OBJECT IDENTIFIER ::= { ntpEntConformance 1 }
ntpEntGroups       OBJECT IDENTIFIER ::= { ntpEntConformance 2  }


ntpEntNTPCompliance MODULE-COMPLIANCE
    STATUS      current
    DESCRIPTION
        "The compliance statement for SNMP entities which use NTP and
         implement the NTP MIB"
    MODULE  -- this module
        MANDATORY-GROUPS {
                           ntpEntObjectsGroup1
        }
        ::= { ntpEntCompliances 1 }

ntpEntSNTPCompliance MODULE-COMPLIANCE
    STATUS      current
    DESCRIPTION
        "The compliance statement for SNMP entities which use SNTP and
         implement the NTP MIB"
    MODULE  -- this module
        MANDATORY-GROUPS {
                           ntpEntObjectsGroup1
        }
        GROUP ntpEntObjectsGroup2


        DESCRIPTION
                "optional notifications for this MIB"
        ::= { ntpEntCompliances 2 }
ntpEntObjectsGroup1 OBJECT-GROUP
    OBJECTS {
              ntpEntSoftwareName
}

   STATUS      current
    DESCRIPTION
        "A collection of objects for the NTP MIB."
    ::= { ntpEntGroups 1 }



ntpEntObjectsGroup2 OBJECT-GROUP
    OBJECTS {

 ntpEntTimeResolution,
 ntpEntTimePrecision,
ntpEntStatusActiveRefSourceId,
ntpStatus
}
STATUS      current
    DESCRIPTION
        "A collection of objects for the NTP MIB."
    ::= { ntpEntGroups 2 }

END
```
Now let's add this mib file to `/etc/snmp/snmpd.conf` as`mibs +NTPv4-MIB`


In order to validate the MIB structure, we can use smilint. 
validate
Run `smilint /usr/share/snmp/mibs/ntp.txt`

![IMG_20230622_103827](https://github.com/hilmiugurpolat/snmp/assets/110428681/f9f5f3a4-62a7-415e-9a91-9a79a83c6f98)

if you don't get any output then mib file is in correct format

The snmptranslate command helps us find OIDs for certain features in NTPv4-MIB.

![IMG_20230622_104920](https://github.com/hilmiugurpolat/snmp/assets/110428681/e9c0f8a2-2140-4313-a8ff-344c827c54fb)

# EXTENDING THE SNMP AGENT

In order to add new functionality to an SNMP agent the agent must be extended. 

https://vincent.bernat.ch/en/blog/2012-extending-netsnmp




to be continued .. 







