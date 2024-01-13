# 윈도우 열거

```powershell
net user mhope #mhope 유저에 대한 정보 확인 
net user mhope /domain #해당 도메인의 mhope 유저에 대한 정보 확인
net view \\s021m005 /all #해당 유저의 모든 파일 보여주기 (숨겨진 공유파일을 확인할때 사용)
whoami /groups #그룹에 대한 정보 출력 
[environment]::OSVersion.Version #OS 버전 출력 
```

```bash
crackmapexec smb -u s.smith -p sT333ve2 --shares 10.10.10.182
#공유가능한 폴더 보는 명령어, 자주 사용하기
```

```powershell
//PowerShell
PS Z:\> [System.Net.WebProxy]::GetDefaultProxy() #시스템 프록시 확인
```
