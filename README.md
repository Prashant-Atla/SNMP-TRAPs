# SNMP-TRAPs
The purpose of this assignment is send traps to the manager of mangers when the device is under observation if two or mare devices are in danger state and atleat one device in fail state

the directory consist of three main files ie trapdaemon.pl which is an perl script used to send traps to the manager of mangershere

index.php 
this is the main web GUI where we can enter the credential of the manager of mangers

pre-requisites 
apache server,MySql,PHP

SNMP Modules which are needed to be installed from CPAN are 
Data::Dumper
DBD::Mysql
DBI
File::Basename
file::spec::Functions
Net::SNMP

install snmpd by "apt-get install snmpd".
  
configuration file to be changed:
##################################
1. Add the following lines to the snmpdtrapd.conf file in the /etc/snmp/snmptrapd.conf:

	authCommunity log,execute,net public 
	disableAuthorization yes
	#doNotLogTraps yes
	snmpTrapdAddr udp:50162
	traphandle 1.3.6.1.4.1.41717.10.* perl /var/www/html/et2536-prat15/assignment3/trapDaemon.pl 
																				 (eg. /var/www/html/et2536-prat15/assign/trapDaemon.pl)
2. Open file snmpd from /etc/default/snmpd and change the line:

	 TRAPDRUN=no to TRAPDRUN=yes

3. Then, use the terminal command "sudo service snmpd restart".
Steps to run this assignment:
-----------------------------

1. Go to the terminal in move to the directory /var/www/html/et2536-prat15/assignment3/ 
   (Move to the path of your working directory configured in your Apache server)

2. Give the trap command:

sudo snmptrap -v 1 -c public 127.0.0.1:50162 .1.3.6.1.4.1.41717.10 10.2.3.4 6 247 '' .1.3.6.1.4.1.41717.10.1 s "" .1.3.6.1.4.1.41717.10.2 i 1

where "" is the FQDN name, 1 is an integer describing the status of device.(0="OK", 1="PROBLEM", 2="DANGER", 3="FAIL")

3. Now, open a web browser and type the following URL: (it is assumed that the working directory is in /var/www/html/)
	 http://localhost/et2536-prat15/assignment3/index.php

4. This page dispalys the status of all the devices. The user can also enter the device credentials of manager in this page.

5. The user can see the traps using "wireshark" or "tcpdump" command.
		
   tcpdump command:
	 sudo tcpdump -n -i wlan0 "dst host 192.168.1.1 and dst port 161"
	 where IP address and port number are of the manager of managers.

	 Wireshark:
   Apply filter as: "ip.addr==192.168.1.1" and start capture on the required interface, can be eth0 or wlan0.
	 where "ip.addr" is the IP address of managers of managers.









