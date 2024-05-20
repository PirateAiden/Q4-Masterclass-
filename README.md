# Q4-Masterclass-

TryHackMe note and progress | 




Notes: 

Nmap Understanding and scripts

#List of all scripts for namp#  
https://nmap.org/book/man-briefoptions.html


#Comand line to search for scripts#
ls -l /usr/share/nmap/scripts/*ftp*:    *FTP is to narrow the search this can vary depending on what script your looking for* 

#Instal Nmap scripts which arent on local machine# [Comand line] 
sudo apt update && sudo apt install nmap


TYPES OF NMAP SCANS !!


#FIN#

FIN scans (-sF) work in an almost identical fashion; however, instead of sending a completely empty packet, a request is sent with the FIN flag (usually used to gracefully close an active connection). Once again, Nmap expects a RST if the port is closed.

#NULL#

As the name suggests, NULL scans (-sN) are when the TCP request is sent with no flags set at all. As per the RFC, the target host should respond with a RST if the port is closed.


#TCP#  

Includes 3-way handshake 
A slow and methodical scan that involves sending a large number of packets to each port. It establishes a full connection by completing a three-way handshake, but it can't distinguish between an unfiltered port and a filtered port with an active service. To perform a TCP Connect scan on the site scanme.nmap.org, you can use the command nmap -sT scanme.nmap.org


If port closed send back RST pakcet to host [action can be same if hiden behind a firewall]

#SYN#

Half-open 
Also known as a half-open scan, this quick and efficient scan indicates open, filtered, and closed port states. It's less likely to be blocked by firewalls because it doesn't complete the full TCP connection. To perform a TCP SYN scan, you can use the command sudo nmap -sS <target>

#UDP#

Unlike TCP, UDP connections are stateless. This means that, rather than initiating a connection with a back-and-forth "handshake", UDP connections rely on sending packets to a target port and essentially hoping that they make it. This makes UDP superb for connections which rely on speed over quality (e.g. video sharing), but the lack of acknowledgement makes UDP significantly more difficult (and much slower) to scan. The switch for an Nmap UDP scan is (-sU)

If UDP packet gets no repsons Nmap refers to the port being open | filterd   [Can distinguish if target has a firewall active]







FIREWALL EVASION !!


Windows firewall prevents all ICMP packets this means that Nmap will register a host with this firewall configuration as dead and not bother scanning it at all. 
[Can be bypssed this with SWITCH -Pn]

[DRAWBACKS TO THIS SWITCH]
which tells Nmap to not bother pinging the host before scanning it. This means that Nmap will always treat the target host(s) as being alive, effectively bypassing the ICMP block; however, it comes at the price of potentially taking a very long time to complete the scan (if the host really is dead then Nmap will still be checking and double checking every specified port).

[Link for more siwtches for firewall evasion]   | Firewall/IDS Evasion and Spoofing |     https://nmap.org/book/man-bypass-firewalls-ids.html


[Note worthy switches]  
-f:- Used to fragment the packets (i.e. split them into smaller pieces) making it less likely that the packets will be detected by a firewall or IDS.
An alternative to -f, but providing more control over the size of the packets: --mtu <number>, accepts a maximum transmission unit size to use for the packets sent. This must be a multiple of 8.
--scan-delay <time>ms:- used to add a delay between packets sent. This is very useful if the network is unstable, but also for evading any time-based firewall/IDS triggers which may be in place.
--badsum:- this is used to generate in invalid checksum for packets. Any real TCP/IP stack would drop this packet, however, firewalls may potentially respond automatically, without bothering to check the checksum of the packet. As such, this switch can be used to determine the presence of a firewall/IDS.





NETWORKING SERVICES !! \\\ NEW ROOM \\\

____________________________________________________________

What is the Server Message Block protocol?

The Server Message Block protocol (SMB protocol) is a client-server communication protocol used for sharing access to files, printers, serial ports and other resources on a network. It can also carry transaction protocols for interprocess communication. Over the years, SMB has been used primarily to connect Windows computers, although most other systems -- such as Linux and macOS -- also include client components for connecting to SMB resources.






Enum4Linux

Enum4linux is a tool used to enumerate SMB shares on both Windows and Linux systems. It is basically a wrapper around the tools in the Samba package and makes it easy to quickly extract information from the target pertaining to SMB. It's installed by default on Parrot and Kali, however if you need to install it, you can do so from the official github.

The syntax of Enum4Linux is nice and simple: "enum4linux [options] ip"

TAG            FUNCTION

-U             get userlist
-M             get machine list
-N             get namelist dump (different from -U and-M)
-S             get sharelist
-P             get password policy information
-G             get group and member list

-a             all of the above (full basic enumeration)



SMB CLIENT 

Because we're trying to access an SMB share, we need a client to access resources on servers. We will be using SMBClient because it's part of the default samba suite. While it is available by default on Kali and Parrot, if you do need to install it, you can find the documentation here.

We can remotely access the SMB share using the syntax:

smbclient //[IP]/[SHARE]

Followed by the tags:

-U [name] : to specify the user

-p [port] : to specify the port

______________________________________________
 PROCCES TO SSH TO A MACHINE YOUR NOT ALLOWED TO BE ON USING SMB

 First run an Nmap scan to find out open ports running SMB then use | smbclient //[IP]/[SHARE] | commnad line to find a share which can be logged into to (e.x.p a share like profiles ect...)   once you SMB into a share look for anything you can use to use to SSH into the target machine. For example lets say you find a file which acknlodeges SHH find out what kind of files Directory SHH has. For the example lets say there a file in which hold an enctrpyted key to gain acces to SHH your going to want to dowload that file with mget* . then go to root machine and chmod 600 [file name] once that is done you SHH with the this command line | ssh -i [file in which key is in] [username]@[Ip] then you have acces to the machine

              ----------------- This is extremly waterd down but idea and pricnple is there --------------------------




What is Telnet?

Telnet is an application protocol which allows you, with the use of a telnet client, to connect to and execute commands on a remote machine that's hosting a telnet server.

The telnet client will establish a connection with the server. The client will then become a virtual terminal- allowing you to interact with the remote host.



------------------------------------------------------------------------------------------------------------




How does Telnet work?

The user connects to the server by using the Telnet protocol, which means entering "telnet" into a command prompt. The user then executes commands on the server by using specific Telnet commands in the Telnet prompt. You can connect to a telnet server with the following syntax: "telnet [ip] [port]"







_-----------_------_------------------_--------_---


What is FTP?

File Transfer Protocol (FTP) is, as the name suggests , a protocol used to allow remote transfer of files over a network. It uses a client-server model to do this




Active vs Passive

The FTP server may support either Active or Passive connections, or both. 

In an Active FTP connection, the client opens a port and listens. The server is required to actively connect to it. 
In a Passive FTP connection, the server opens a port and listens (passively) and the client connects to it. 


HOW TO EXPLOUT FTP
_----------------_

Lets assume that we ran an Nmap scan and we see that Port 21 is running a FTP serive and we see from the scan that its version is ftp-anon indacting that it can be logged into anonymously, we use this information to log on and find that there a file laying around called PUBLIC_NOTICE.txt which is letting everyone know that there will be a service on the server. Whoever wrote this used FTP to share it accros a large platform and at the bottom of this note he left his name, "Mike" which we can safly assume is his log in username once we have this infomration we run a Hydra brute force attack to crack his password [SYNATX for hydra hydra -t 4 -l dale -P /usr/share/wordlists/rockyou.txt -vV 10.10.10.6 ftp]
Ip will vary from thr machine you are attacking along with the -l which is the username [so from dale ---> mike]. once this is completed we have his password and full access for the machine and sever.  

Syntax to FTP to a server is [ ftp [ip] ]

--------------------------- Water down but idea and excution along side with synatx is there ---------------------------------------






What is NFS?

NFS stands for "Network File System" and allows a system to share directories and files with others over a network. By using NFS, users and programs can access files on remote systems almost as if they were local files. It does this by mounting all, or a portion of a file system on a server. The portion of the file system that is mounted can be accessed by clients with whatever privileges are assigned to each file.


______________________


SQL 

MySQL is likely not going to be the first point of call when getting initial information about the server. You can, as we have in previous tasks, attempt to brute-force default account passwords if you really don't have any other information; however, in most CTF scenarios, this is unlikely to be the avenue you're meant to pursue.




Typically, you will have gained some initial credentials from enumerating other services that you can then use to enumerate and exploit the MySQL service. As this room focuses on exploiting and enumerating the network service, for the sake of the scenario, we're going to assume that you found the credentials: "root:password" while enumerating subdomains of a web server. After trying the login against SSH unsuccessfully, you decide to try it against MySQL.


---- Note; SQL is famously used by facebook ----



Procces to ssh into machine using SQL:


you are going to want to use mettasploit and being able to naviagte the framework is crucial. First step is figuring out any users on the server which arent there by default. we found carl which seems unusual, and using this exploit from metaploit called mysql_hashdump we now have his hashed password. copy this into a file on your local machine to make it eaiser and run "john" ((which is a tool used to crack hases)) synatx ("john [file name]") which cracks his password. now we use out past knoledge of our Nmap to know theres an acticve ssh running then ssh using the info we just gained 











-----------------------------------------------------------------


New room !!!!


Metasploit fundamentals 



Encoders

Encoders will allow you to encode the exploit and payload in the hope that a signature-based antivirus solution may miss them.

Signature-based antivirus and security solutions have a database of known threats. They detect threats by comparing suspicious files to this database and raise an alert if there is a match. Thus encoders can have a limited success rate as antivirus solutions can perform additional checks.




Evasion

While encoders will encode the payload, they should not be considered a direct attempt to evade antivirus software. On the other hand, “evasion” modules will try that, with more or less success.




Exploits

Exploits, neatly organized by target system.
____
Examples !
______

root@ip-10-10-135-188:/opt/metasploit-framework/embedded/framework/modules# tree -L 1 exploits/
exploits/
├── aix
├── android
├── apple_ios
├── bsd
├── bsdi
├── dialup
├── example_linux_priv_esc.rb
├── example.py
├── example.rb
├── example_webapp.rb
├── firefox
├── freebsd
├── hpux
├── irix
├── linux
├── mainframe
├── multi
├── netware
├── openbsd
├── osx
├── qnx
├── solaris
├── unix
└── windows

20 directories, 4 files




NOPs

NOPs (No OPeration) do nothing, literally. They are represented in the Intel x86 CPU family with 0x90, following which the CPU will do nothing for one cycle. They are often used as a buffer to achieve consistent payload sizes.




Payloads

Payloads are codes that will run on the target system.

Exploits will leverage a vulnerability on the target system, but to achieve the desired result, we will need a payload. Examples could be; getting a shell, loading a malware or backdoor to the target system, running a command, or launching calc.exe 










