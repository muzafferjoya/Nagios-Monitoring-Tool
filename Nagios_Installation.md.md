**Nagios Core Installation on Centos Step by Step**

Nagios core is an most popular and enterprise open source monitoring tool. Using this monitoring tool you can monitor any device in this world even UPS / Battery status. This tool will support almost all the scripting languages.

Prepare your Linux server to install Nagios monitoring tool


**Prerequisites:**

* Nagios code is written in C language so you required an gcc++ compiler packages to compile and install Nagios.
* Required wget package to download the nagios tool directly from web to server
* To untar  the package required Rar / Zip RPM’s
* Web / Apache / Httpd service to host the Nagios
* Install php and perl packages to run Nagios plugins

Step: 1 -> Firewall stop and disable

$ systemctl stop firewalld

$ systemctl disable firewalld

Step:2 -> SELINUX

$ cat /etc/selinux/config | grep SELINUX=e

$ sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config

$ cat /etc/selinux/config | grep SELINUX=d

Step:3 -> Check SESTATUS

$ sestatus

reboot the system

and again check sestatus 


**Now, Installing Nagios Core**

$ yum install -y wget httpd php gcc glibc glibc-common gd gd-devel make net-snmp perl perl-devel openssl

$ tar -xvf nagios-4.4.3

before installing add user,group

**Create Nagios User and Nagcmd group**

$ useradd nagios                	// Creating Nagios User

$ groupadd nagcmd                	// Creating Nagcmd Group

$ usermod -a -G nagcmd nagios   // Adding nagios user to nagcmd group

after doing this step let's start installing Nagios

**Compiling Nagios Code**

$ cd nagios-4.4.3

$ ./configure --with-command-group-nagcmd

$ make all						// To make the package

$ make install 					// To installed the complied 								   package

$ make install-init					// To install init script in 									   /etc/rc.d/init.d

$ make install-config				// To Generate the config files

$ make install-commandmode		//To install the command mode 							enable



$ make install-webconf			// To Generate the Web config file

**Copy the Event handler directory and change its ownership**

$ cp -rvf contrib/eventhandlers/ /usr/local/nagios/libexec/

**Change ownership of copied directory using below command**

$ chown -R nagios:nagcmd /usr/local/nagios/libexec/eventhandlers


**Create nagiosadmin user and Generate the password to Web Login**

$ htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin

**Start the httpd and nagios services**

$ systemctl start httpd.service 

$ systemctl start nagios.service

**Check the services status**

$ systemctl status httpd.service 

$ systemctl status nagios.service

**Access the Nagios using web browser**

open the browser and type <http://IPADDRESS/nagios>


**Install Nagios plugins on Nagios Server**



Nagios plugins will play main role to monitor the service. Its very easy to install the nagios plugins.

nagios plugins are locate yet /usr/local/nagios/libexec/

$ wget <http://www.nagios-plugins.org/download/nagios-plugins-2.3.3.tar.gz>

extract the package

$ tar -xzvf nagios-plugins-2.0.3.tar.gz

**Change to directory to install plugins**

$ cd nagios-plugins-2.0.3 

$ ./configure --with-nagios-user=nagios --with-nagios-group=nagcmd 

$ make 

$ make install



























