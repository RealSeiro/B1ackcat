# Cmdkey 저장된 자격증명

* [cmdkey](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/cmdkey) 명령은 저장된 사용자 이름과 비밀번호를 만들고, 나열하고, 삭제하는 데 사용할 수 있음
  * 사용자는 특정 호스트에 대한 자격 증명을 저장하거나 원격 데스크톱을 사용하여 비밀번호를 입력하지 않고도 원격 호스트에 연결할 수 있도록 터미널 서비스 연결에 대한 자격 증명을 저장하는 데 사용할 수 있음
  * &#x20;이렇게 하면 다른 사용자가 있는 다른 시스템으로 측면 이동하거나 현재 호스트에 대한 권한을 에스컬레이션하여 다른 사용자를 위해 저장된 자격 증명을 활용할 수 있음

&#x20; 저장된 자격 증명 목록 나열

```
C:\Users\security>cmdkey /list

Currently stored credentials:

    Target: Domain:interactive=ACCESS\Administrator
    Type: Domain Password
    User: ACCESS\Administrator
```

* 호스트에 RDP를 시도할 때 저장된 자격 증명이 사용됨&#x20;

<figure><img src="https://academy.hackthebox.com/storage/modules/67/cmdkey_rdp.png" alt="image"><figcaption></figcaption></figure>

* 또한 `runas를` 사용하여 자격 증명을 재사용하여 해당 사용자로 리버스 셸을 보내거나, 바이너리를 실행하거나, 다음과 같은 명령으로 PowerShell 또는 CMD 콘솔을 실행할 수도 있음

**다른 사용자로 명령 실행**

```
PS C:\htb> runas /savecred /user:inlanefreight\bob "COMMAND HERE"
```
