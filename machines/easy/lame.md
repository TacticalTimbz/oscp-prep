# Initial Scan

```
orlando@tacticaltimbz  ~/Downloads  sudo nmap -sV -O 10.10.10.3
[sudo] password for orlando: 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-10-24 02:10 EDT
Nmap scan report for 10.10.10.3
Host is up (0.042s latency).
Not shown: 996 filtered tcp ports (no-response)
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 2.3.4
22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 2.6.23 (92%), Dell Integrated Remote Access Controller (iDRAC6) (90%), Linksys WET54GS5 WAP, Tranzeo TR-CPQ-19f WAP, or Xerox WorkCentre Pro 265 printer (90%), Linux 2.4.21 - 2.4.31 (likely embedded) (90%), Linux 2.4.7 (90%), Citrix XenServer 5.5 (Linux 2.6.18) (90%), Linux 2.6.27 - 2.6.28 (90%), Linux 2.6.8 - 2.6.30 (90%), Dell iDRAC 6 remote access controller (Linux 2.6) (90%), Supermicro IPMI BMC (Linux 2.6.24) (90%)
No exact OS matches for host (test conditions non-ideal).
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.03 seconds
 orlando@tacticaltimbz  ~/Downloads  
```

