BASIC COMMANDS
Experiment: 1
Aim: Understand basic commands

    • IPCONFIG COMMAND

To see the IP address Syntax: ipconfig
• PING COMMAND
To see the existing the IP Syntax: ping 192.168.87.1
• TO UPDATE
Sudo apt update
• TO INSTALL
Sudo apt install openSSH.server
• TO PRINT THE SSH SERVER STATUS
Sudo systemctl status ssh
• TO EDIT SETTINGS
Sudo nano /etc/ssh/sshd-config
• TO RESTART
Suso service ssh restart
• TO TEST
ssh localhost
• UPTIME COMMAND
In Linux uptime command shows since how long your system is running and the number of users are currently logged in and also displays load average for 1,5- and 15- minutes intervals.
Syntax: uptime
• W COMMAND
It will display users currently logged in and their process along-with shows load averages. Also shows the login name, tty name, remote host, login time, idle time, JCPU, PCPU, command and processes.
Syntax: w
• USERS COMMAND
Users command displays currently logged in users. This command don’t have other
parameters other than help and version. Syntax: users
• WHO COMMAND
who command simply return user name, date, time and host information who command is similar to w command. Unlike w command who doesn’t print what users are doing. Let’s illustrate and see the different between who and w commands.
Syntax: who
• WHOAMI COMMAND
whoami command print the name of current user. You can also use “who am i” command to display the current user. If you are logged in as a root using sudo command “whoami” command return root as current user. Use “who am i” command if you want to know the exact user logged in.
Syntax: whoami
• LS COMMAND
ls command display list of files in human readable format. Syntax: ls -l
• CRONTAB COMMAND
List schedule jobs for current user with crontab command and -l option. Syntax: crontab -l
• LESS COMMAND
less command allows quickly view file. You can page up and down. Press ‘q‘ to quit
from less window. Syntax: less install.log
• MORE COMMAND
more command allows quickly view file and shows details in percentage. You can page up and down. Press ‘q ‘to quit out from more window.
Syntax: more install.log
• CP COMMAND
Copy file from source to destination preserving same mode. Syntax: cp -p fileA fileB
• MV COMMAND
Rename fileA to fileB. -i options prompt before overwrite. Ask for confirmation if exist already.
Syntax: mv -i fileA fileB
• CAT COMMAND
cat command used to view multiple file at the same time. Syntax: cat fileA fileB
• CD COMMAND (CHANGE DIRECTORY)
with cd command (change directory) it will goes to fileA directory. Syntax: cd /fileA

Result:
All the commands have been executed and the output has been obtained successfully.

SAMBA SHARE
Experiment: 2
Aim: Installation and configuration of Samba share.
Description:

SAMBA

One of the most common ways to network Ubuntu and Windows computers is to configure Samba as a File Server. This section covers setting up a Samba server to share files with Windows clients.
The server will be configured to share files with any client on the network without prompting for a password. If your environment requires stricter Access Controls see Share Access Control
Port No: 139
Package name: samba
Configuration file: /etc/samba/smb.conf.

Procedure:

    1. To install Samba, we can run:

$sudo apt update
$sudo apt install samba 2. We can check if the installation was successful by running:
$whereis samba
    3. Now that Samba is installed, we need to create a directory for it to share:
$mkdir /home/&lt;username&gt;/sambashare/
The command above creates a new folder samba share in our home directory which we will share later.The configuration file for Samba is located at
/etc/samba/smb.conf. To add the new directory as a share,we edit the file by running:
$sudo nano /etc/samba/smb.conf
At the bottom of the file, add the following lines:

[sambashare]
comment = Samba on Ubuntu
path = /home/username/sambashare read only = no
browsable = yes 4. Then press Ctrl-O to save and Ctrl-X to exit from the nano text editor. 5. Now that we have our new share configured, save it and restart Samba for it to take effect:
$sudo service smbd restart
    6. Update the firewall rules to allow Samba traffic:
$sudo ufw allow samba
SETTING UP USER ACCOUNTS AND CONNECTING TO SHARE

    7. Since Samba doesn’t use the system account password, we need to set up a Samba

password for our user account:
$sudo smbpasswd -a username

CONNECTING TO SHARE

    8. On Ubuntu: Open up the default file manager and click Connect to Server then enter: Connecting to samba via smb://127.0.0.1/sambashare

Note: ip-address is the Samba server IP address and sambashare is the name of the
share. You’ll be prompted for your credentials. Enter them to connect

DNS
Experiment: 3
Aim: To create and configure DNS Server
Description:
DNS Server

A DNS server is a computer server that contains a database of public IP addresses and their associated hostnames, and in most cases, serves to resolve, or translate, those common names to IP addresses as requested.

Port No: 53
Package name: bind9

Configuration file: /etc/bind/named.conf. (Primary configuration file),/etc/bind/db.root (root nameservers)

Procedure:
CASHING NAMESERVER

When configured as a caching nameserver BIND9 will find the answer to name queries and
remember the answer when the domain is queried again.

        1. Install bind9 by typing

$sudo apt install bind9
$sudo apt install dnsutils

        2. The default configuration is set up to act as a caching server. All that is required is simply

adding the IP Addresses of your ISP's DNS servers. Simply uncomment and edit the following in /etc/bind/named.conf.options:

        3. Restart it by typing

$sudo systemctl restart bind9.service
PRIMARY MASTER
As a primary master server BIND9 reads the data for a zone from a file on it's host and is authoritative for that zone.

Forward zone file

To add a DNS zone to BIND9, turning BIND9 into a Primary Master server, the first step is to edit /etc/bind/named.conf.local:

$sudo cp /etc/bind/db.local /etc/bind/db.example.com
$sudo systemctl restart bind9.service

Reverse Zone File
Now that the zone is set up and resolving names to IP Addresses, a Reverse zone
needs to be added to allows DNS to resolve an address to a name. 1. Edit /etc/bind/named.conf.local 2. Now create the /etc/bind/db.192 file:
$sudo cp /etc/bind/db.127 /etc/bind/db.192

    3. edit /etc/bind/db.192 changing the basically the same options as

/etc/bind/db.example.com:

    4. After creating the reverse zone file restart BIND9:

$sudo systemctl restart bind9.service
    5. Check the status
$Sudo service bind9 status 6. Check if nslookup can resolve

$nslookup ftp.example.com
$nslookup ubuntu.example.com 7. Gather information about your DNS server
$dig ubuntu.example.com
$dig www.example.com
$dig ftp.example.com

FTP
Experiment : 4
Aim : To create and configure FTP Server
Description :
FTP Server

File Transfer Protocol (FTP) is a TCP protocol for downloading files between computers. In the past, it has also been used for uploading but, as that method does not use encryption, user credentials as well as data transferred in the clear and are easily intercepted. So if you are here looking for a way to upload and download files securely,

FTP works on a client/server model. The server component is called an FTP daemon. It continuously listens for FTP requests from remote clients. When a request is received, it manages the login and sets up the connection. For the duration of the session it executes any of commands sent by the FTP client
Port No: 21
Package name: vsftpd
Configuration file: /etc/vsftpd.conf

Procedure: 1. Install the vsftpd - FTP Server Installation in the ubuntu operating system
$sudo apt install vsftpd
    2. By default vsftpd is not configured to allow anonymous download. If you wish to enable anonymous download edit /etc/vsftpd.conf by changing:
$anonymous_enable=YES 3. During installation a ftp user is created with a home directory of /srv/ftp. This is the default FTP directory.
If you wish to change this location, to /srv/files/ftp for example, simply create a directory in another location and change the ftp user’s home directory:
$sudo mkdir -p /srv/files/ftp
$sudo usermod -d /srv/files/ftp ftp 4. After making the change restart vsftpd:
$ sudo service vsftpd restart

    5. User Authenticated FTP Configuration

By default vsftpd is configured to authenticate system users and allow them to download files. If you want users to be able to upload files, edit /etc/vsftpd.conf
$write_enable=YES

    6. Now restart vsftpd:

$ sudo service vsftpd restart

    7. Securing FTP

There are options in /etc/vsftpd.conf to help make vsftpd more secure.
$chroot_local_user=YES
$chroot_list_enable=YES
$chroot_list_file=/etc/vsftpd.chroot_list

    8. After uncommenting the above options, create a /etc/vsftpd.chroot_list containing a list of users one per line.

    9. Then restart vsftpd:

$sudo service vsftpd restart

    10. To configure FTPS, edit /etc/vsftpd.conf and at the bottom add:

$ssl_enable=YES

    11. Then check the vsftpd status

$sudo service vsftpd status

    12. Now connect to ftp by the command

$ftp -p 192.168.234.128

    13. Now install filezilla in ubuntu and open the filezilla and specify the ip address and port number of the ftp server then click connect






    SQUID

Experiment: 5
Aim: To create and configure Squid -proxy server
Description:

SQUID – PROXY SERVER

Squid is a full-featured web proxy cache server application which provides proxy and cache services for HyperText Transport Protocol (HTTP), File Transfer Protocol (FTP), and other popular network protocols. Squid can implement caching and proxying of Secure Sockets Layer (SSL) requests and caching of Domain Name Server (DNS) lookups, and perform transparent caching. Squid also supports a wide variety of caching protocols, such as Internet Cache Protocol (ICP), the HyperText Caching Protocol (HTCP), the Cache Array Routing Protocol (CARP), and the Web Cache Coordination Protocol (WCCP).
The Squid proxy cache server is an excellent solution to various proxy and caching server needs, and scales from the branch office to enterprise-level networks while providing extensive, granular access control mechanisms, and monitoring of critical parameters via the Simple Network Management Protocol (SNMP). When selecting a computer system for use as a dedicated Squid caching proxy server for many users ensure it is configured with a large amount of physical memory as Squid maintains an in-memory cache for increased performance.

Port No: 3128
Package name: squid
Configuration file: /etc/squid/squid.conf

Procedure: 1. At a terminal prompt, enter the following command to install the Squid server:
$sudo apt install squid 2. Squid is configured by editing the directives contained within the /etc/squid/squid.conf configuration file. 3. Change the access as shown below:
acl localnet src 192.168.234.139(your ip address)
acl blocksite dstdomain &quot;/etc/squid/blocksite&quot; http_access deny blocksite
http_access allow localnet #http_access deny all http_access allow all

    4. To block access to the website we must configure using &quot;/etc/squid/blocksite”

we edit the file by running:
$cd /etc/squid
$sudo gedit blocksite

    5. Add the websites to block:

in this case, I am blocking youtube, facebook, google 6. To check the actual functioning of the proxy server go to the browser and click settings, search proxy in connection settings.

    7. To configure Proxy access to the internet
    8. Select Manual Proxy configuration
    9. Type your HTTP Proxy(IP Address) and Port number as 3128.
    10. Select SOCKS v5

CONNECTING TO WEBSITE

    11. Search for the blocked websites
    12. Access is denied to the above websites





    SSH

Experiment:6
Aim: Installation of Open SSH between two ubuntu machines.
Description:

Remote File Sharing using SSH

OpenSSH is a powerful collection of tools for the remote control of, and transfer of data between, networked computers. You will also learn about some of the configuration settings possible with the OpenSSH server application and how to change them on your Ubuntu system.
OpenSSH is a freely available version of the Secure Shell (SSH) protocol family of tools for remotely controlling, or transferring files between computers. Traditional tools used to accomplish these functions, such as telnet or rcp, are insecure and
transmit the user’s password in cleartext when used. OpenSSH provides a server daemon and client tools to facilitate secure, encrypted remote control and file transfer operations, effectively replacing the legacy tools.
Port No: 22
Package name: openssh-client
Configuration file: /etc/ssh/sshd_config

Procedure: 1. create two EC2 instance of ubuntu ssh client and ssh server 2. Create the password for the instance of ssh server by $sudo passwd ubuntu
    3. Now check whether the ssh server is running by the command $sudo service ssh status
    4. configure the sshd_config file by the following command $sudo vim
/etc/ssh/sshd_config and include the following changes PasswordAuthentication yes , KbdInteractiveAuthentication no ,KerberosGetAFSToken no
    5. Now check the status of the ssh server by the command $sudo service ssh status
    6. Now create a text file by the command $touch text.txt
    7. Now log in to the ssh_client and create a ssh_keygen by the command
$ssh_keygen 8. Now copy the ssh_keygen form the ssh_client $ssh-copy-id ubuntu@privateip 9. Now restart the client machine 10. Then connect to the ssh_server by ssh_client 11. then type ls you will be prompted with the screen with your text file which you have created
