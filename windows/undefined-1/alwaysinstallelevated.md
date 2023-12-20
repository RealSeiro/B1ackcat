# AlwaysInstallElevated

이 레지스트리 키는 모든 권한이 있는 사용자가 `.msi` 파일을 설치할 수 있음을 Windows에 알려주는 NT AUTHORITY\SYSTEM입니다. 따라서 악성 `.msi` 파일을 만들어 실행하기만 하면 됩니다.

`msfvenon을` 사용하여 MSI 인스톨러를 생성하겠습니다. [이더리움에서는](https://0xdf.gitlab.io/2019/03/09/htb-ethereal.html#create-msi) 이 과정을 수동으로 보여드렸지만, 이 과정은 번거롭기 때문에 여기서는 `msfvenom이` 작동합니다. `nc로` 잡을 수 있는 리버스 셸 페이로드를 사용하겠습니다:

