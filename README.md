# Q4-Masterclass-
Not sure yet  

TryHackMe note and progress | 




Notes: 

Nmap Understanding and scripts

#List of all scripts for namp#  
https://nmap.org/book/man-briefoptions.html


#Comand line to search for scripts#
ls -l /usr/share/nmap/scripts/*ftp*:

#Instal Nmap scripts which arent on local machine# [Comand line]
sudo apt update && sudo apt install nmap

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


