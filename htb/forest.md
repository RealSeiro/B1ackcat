# Forest

## Nmap

```bash
# Nmap 7.94SVN scan initiated Wed Jun 19 10:26:00 2024 as: nmap -sC -sV -p 88,135,139,389,445,464,593,636,3268,3269,5403,5985,9389,47001 -oA nmap 10.10.10.161
Nmap scan report for 10.10.10.161
Host is up (0.073s latency).

PORT      STATE  SERVICE       VERSION
88/tcp    open   kerberos-sec  Microsoft Windows Kerberos (server time: 2024-06-19 10:26:13Z)
135/tcp   open   msrpc         Microsoft Windows RPC
139/tcp   open   netbios-ssn   Microsoft Windows netbios-ssn
#smb, rpc 열거
389/tcp   open   ldap          Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
#ldap 열거
445/tcp   open   microsoft-ds  Windows Server 2016 Standard 14393 microsoft-ds (workgroup: HTB)
464/tcp   open   kpasswd5?
593/tcp   open   ncacn_http    Microsoft Windows RPC over HTTP 1.0
#웹 X
636/tcp   open   tcpwrapped
3268/tcp  open   ldap          Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
3269/tcp  open   tcpwrapped
5403/tcp  closed hpoms-ci-lstn
5985/tcp  open   http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
#Winrm 존재
9389/tcp  open   mc-nmf        .NET Message Framing
47001/tcp open   http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Service Info: Host: FOREST; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: mean: 2h20m06s, deviation: 4h02m30s, median: 5s
| smb2-time: 
|   date: 2024-06-19T10:26:23
|_  start_date: 2024-06-19T04:15:07
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
|   Computer name: FOREST
|   NetBIOS computer name: FOREST\x00
|   Domain name: htb.local
|   Forest name: htb.local
|   FQDN: FOREST.htb.local
|_  System time: 2024-06-19T03:26:21-07:00

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Jun 19 10:26:25 2024 -- 1 IP address (1 host up) scanned in 24.25 seconds
```



열거

<pre class="language-bash"><code class="lang-bash"><strong>#smb but 실패
</strong><strong>┌──(root㉿kali-container-upgrade)-[~/Downloads/HTB/answer/Forest]
</strong>└─# crackmapexec smb 10.10.10.161 -u '' -p '' --shares
SMB         10.10.10.161    445    FOREST           [*] Windows Server 2016 Standard 14393 x64 (name:FOREST) (domain:htb.local) (signing:True) (SMBv1:True)
SMB         10.10.10.161    445    FOREST           [+] htb.local\: 
SMB         10.10.10.161    445    FOREST           [-] Error enumerating shares: STATUS_ACCESS_DENIED

#rpcclient but 
┌─ (root㉿kali-container-upgrade)-[~/Downloads/HTB/answer/Forest]
└─# rpcclient -U "" -N 10.10.10.161
rpcclient $> srvinfo
do_cmd: Could not initialise srvsvc. Error was NT_STATUS_ACCESS_DENIED
rpcclient $> enumdomains
name:[HTB] idx:[0x0]
name:[Builtin] idx:[0x0]
rpcclient $> querydominfo
Domain:         HTB
Server:
Comment:
Total Users:    106
Total Groups:   0
Total Aliases:  0
Sequence No:    1
Force Logoff:   -1
Domain Server State:    0x1
Server Role:    ROLE_DOMAIN_PDC
Unknown 3:      0x1
rpcclient $> netshareenumall
do_cmd: Could not initialise srvsvc. Error was NT_STATUS_ACCESS_DENIED
rpcclient $> enumdomusers
user:[Administrator] rid:[0x1f4]
user:[Guest] rid:[0x1f5]
user:[krbtgt] rid:[0x1f6]
user:[DefaultAccount] rid:[0x1f7]
user:[$331000-VK4ADACQNUCA] rid:[0x463]
user:[SM_2c8eef0a09b545acb] rid:[0x464]
user:[SM_ca8c2ed5bdab4dc9b] rid:[0x465]
user:[SM_75a538d3025e4db9a] rid:[0x466]
user:[SM_681f53d4942840e18] rid:[0x467]
user:[SM_1b41c9286325456bb] rid:[0x468]
user:[SM_9b69f1b9d2cc45549] rid:[0x469]
user:[SM_7c96b981967141ebb] rid:[0x46a]
user:[SM_c75ee099d0a64c91b] rid:[0x46b]
user:[SM_1ffab36a2f5f479cb] rid:[0x46c]
user:[HealthMailboxc3d7722] rid:[0x46e]
user:[HealthMailboxfc9daad] rid:[0x46f]
user:[HealthMailboxc0a90c9] rid:[0x470]
user:[HealthMailbox670628e] rid:[0x471]
user:[HealthMailbox968e74d] rid:[0x472]
user:[HealthMailbox6ded678] rid:[0x473]
user:[HealthMailbox83d6781] rid:[0x474]
user:[HealthMailboxfd87238] rid:[0x475]
user:[HealthMailboxb01ac64] rid:[0x476]
user:[HealthMailbox7108a4e] rid:[0x477]
user:[HealthMailbox0659cc1] rid:[0x478]
user:[sebastien] rid:[0x479]
user:[lucinda] rid:[0x47a]
user:[svc-alfresco] rid:[0x47b]
user:[andy] rid:[0x47e]
user:[mark] rid:[0x47f]
user:[santi] rid:[0x480]
user:[zax] rid:[0x2581]
rpcclient $> 

#유저목록
Administrator
Guest
krbtgt








</code></pre>
