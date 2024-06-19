# FootPrinting

### 인프라 기반 열거



| **명령**                                                              | **설명**                           |
| ------------------------------------------------------------------- | -------------------------------- |
| `curl -s https://crt.sh/\?q\=<target-domain>\&output\=json \| jq .` | 인증서 투명성.                         |
| `for i in $(cat ip-addresses.txt);do shodan host $i;done`           | Shodan을 사용하여 목록의 각 IP 주소를 검색합니다. |

***

### 호스트 기반 열거



**FTP**



| **명령**                                                    | **설명**                               |
| --------------------------------------------------------- | ------------------------------------ |
| `ftp <FQDN/IP>`                                           | 대상의 FTP 서비스와 상호 작용합니다.               |
| `nc -nv <FQDN/IP> 21`                                     | 대상의 FTP 서비스와 상호 작용합니다.               |
| `telnet <FQDN/IP> 21`                                     | 대상의 FTP 서비스와 상호 작용합니다.               |
| `openssl s_client -connect <FQDN/IP>:21 -starttls ftp`    | 암호화된 연결을 사용하여 대상의 FTP 서비스와 상호 작용합니다. |
| `wget -m --no-passive ftp://anonymous:anonymous@<target>` | 대상 FTP 서버에서 사용 가능한 모든 파일을 다운로드합니다.   |

smb



| **명령**                                            | **설명**                        |
| ------------------------------------------------- | ----------------------------- |
| `smbclient -N -L //<FQDN/IP>`                     | SMB에서 Null 세션 인증.             |
| `smbclient //<FQDN/IP>/<share>`                   | 특정 SMB 공유에 연결합니다.             |
| `rpcclient -U "" <FQDN/IP>`                       | RPC를 사용하여 대상과 상호 작용합니다.       |
| `samrdump.py <FQDN/IP>`                           | Impacket 스크립트를 사용한 사용자 이름 열거. |
| `smbmap -H <FQDN/IP>`                             | SMB 공유를 열거합니다.                |
| `crackmapexec smb <FQDN/IP> --shares -u '' -p ''` | 널 세션 인증을 사용하여 SMB 공유를 열거합니다.  |
| `enum4linux-ng.py <FQDN/IP> -A`                   | enum4linux를 사용한 SMB 열거.       |

**NFS**



| **명령**                                                    | **설명**                                    |
| --------------------------------------------------------- | ----------------------------------------- |
| `showmount -e <FQDN/IP>`                                  | 사용 가능한 NFS 공유를 표시합니다.                     |
| `mount -t nfs <FQDN/IP>:/<share> ./target-NFS/ -o nolock` | 특정 NFS share.umount ./target-NFS를 마운트합니다. |
| `umount ./target-NFS`                                     | 특정 NFS 공유를 마운트 해제합니다.                     |

**DNS**



| **명령**                                                                                                        | **설명**                  |
| ------------------------------------------------------------------------------------------------------------- | ----------------------- |
| `dig ns <domain.tld> @<nameserver>`                                                                           | 특정 네임서버에 대한 NS 요청입니다.   |
| `dig any <domain.tld> @<nameserver>`                                                                          | 특정 네임서버에 대한 모든 요청.      |
| `dig axfr <domain.tld> @<nameserver>`                                                                         | 특정 네임서버에 대한 AXFR 요청입니다. |
| `dnsenum --dnsserver <nameserver> --enum -p 0 -s 0 -o found_subdomains.txt -f ~/subdomains.list <domain.tld>` | 하위 도메인 무차별 대입.          |

**SMTP**



| **명령**                | **설명** |
| --------------------- | ------ |
| `telnet <FQDN/IP> 25` |        |

**IMAP/POP3**



| **명령**                                                 | **설명**                        |
| ------------------------------------------------------ | ----------------------------- |
| `curl -k 'imaps://<FQDN/IP>' --user <user>:<password>` | cURL을 사용하여 IMAPS 서비스에 로그인합니다. |
| `openssl s_client -connect <FQDN/IP>:imaps`            | IMAPS 서비스에 연결합니다.             |
| `openssl s_client -connect <FQDN/IP>:pop3s`            | POP3 서비스에 연결합니다.              |

**SNMP**



| **명령**                                            | **설명**                     |
| ------------------------------------------------- | -------------------------- |
| `snmpwalk -v2c -c <community string> <FQDN/IP>`   | snmpwalk를 사용하여 OID를 쿼리합니다. |
| `onesixtyone -c community-strings.list <FQDN/IP>` | SNMP 서비스의 무차별 커뮤니티 문자열입니다. |
| `braa <community string>@<FQDN/IP>:.1.*`          | 무차별 SNMP 서비스 OID.          |

**MySQL**



| **명령**                                      | **설명**            |
| ------------------------------------------- | ----------------- |
| `mysql -u <user> -p<password> -h <FQDN/IP>` | MySQL 서버에 로그인합니다. |

**MSSQL**



| **명령**                                          | **설명**                             |
| ----------------------------------------------- | ---------------------------------- |
| `mssqlclient.py <user>@<FQDN/IP> -windows-auth` | Windows 인증을 사용하여 MSSQL 서버에 로그인합니다. |

**IPMI**



| **명령**                                         | **설명**          |
| ---------------------------------------------- | --------------- |
| `msf6 auxiliary(scanner/ipmi/ipmi_version)`    | IPMI 버전 감지.     |
| `msf6 auxiliary(scanner/ipmi/ipmi_dumphashes)` | IPMI 해시를 덤프합니다. |

**리눅스 원격 관리**



| **명령**                                                      | **설명**                          |
| ----------------------------------------------------------- | ------------------------------- |
| `ssh-audit.py <FQDN/IP>`                                    | 대상 SSH 서비스에 대한 원격 보안 감사.        |
| `ssh <user>@<FQDN/IP>`                                      | SSH 클라이언트를 사용하여 SSH 서버에 로그인합니다. |
| `ssh -i private.key <user>@<FQDN/IP>`                       | 개인 키를 사용하여 SSH 서버에 로그인합니다.      |
| `ssh <user>@<FQDN/IP> -o PreferredAuthentications=password` | 비밀번호 기반 인증을 시행합니다.              |

**Windows 원격 관리**



| **명령**                                                        | **설명**                   |
| ------------------------------------------------------------- | ------------------------ |
| `rdp-sec-check.pl <FQDN/IP>`                                  | RDP 서비스의 보안 설정을 확인하십시오.  |
| `xfreerdp /u:<user> /p:"<password>" /v:<FQDN/IP>`             | Linux에서 RDP 서버에 로그인합니다.  |
| `evil-winrm -i <FQDN/IP> -u <user> -p <password>`             | WinRM 서버에 로그인합니다.        |
| `wmiexec.py <user>:"<password>"@<FQDN/IP> "<system command>"` | WMI 서비스를 사용하여 명령을 실행합니다. |

**오라클 TNS**



| **명령**                                                                                                               | **설명**                                                   |
| -------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| `./odat.py all -s <FQDN/IP>`                                                                                         | 다양한 스캔을 수행하여 Oracle 데이터베이스 서비스 및 해당 구성 요소에 대한 정보를 수집합니다. |
| `sqlplus <user>/<pass>@<FQDN/IP>/<db>`                                                                               | Oracle 데이터베이스에 로그인합니다.                                   |
| `./odat.py utlfile -s <FQDN/IP> -d <db> -U <user> -P <pass> --sysdba --putFile C:\\insert\\path file.txt ./file.txt` | Oracle RDBMS를 사용하여 파일을 업로드합니다.                           |
