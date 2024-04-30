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
