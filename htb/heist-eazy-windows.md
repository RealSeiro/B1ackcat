# Heist(Eazy, Windows)

Nmap : 80, 135, 445, 5985

### http/80

* Cisco
  * ios-1/ stealth1agent&#x20;
  * rout3r/$uperP@ssword
  * admin/Q4)sJu\Y8qz\*A3?d
* user/password
  * Hazard
  * SUPPORTDESK
  * ios-1/ stealth1agent&#x20;
  * rout3r/$uperP@ssword
  * admin/Q4)sJu\Y8qz\*A3?d
* SMB 무차별대입시도 -> 성공
  * Hazard:stealth1agent
  * but smb 열거 불가능 Why? 오로지 IPC$에서만 읽기 권한있음
* Winrm 무차별 대입 시도 - 실패&#x20;
  * 내가 가진 정보 : SMB 크래딧 정보뿐, 웹사이트 로그인 불가, winrm 로그인 불가, 135포트는 지금 상황에선 쓸모가 없음
  * SMB 크래딧을 가지고 SID 공격 or rpcclient 열거
    * SMB SID 열거를 통해 로컬 사용자 목록 획득&#x20;
    * rpcclient로도 사용자 목록 획득 가능
  * 얻은 사용자 목록으로 smb login, winrm login 로그인 시도
    * Chase:Q4)sJu\Y8qz\*A3?d 성공
* Winrm 열거&#x20;
  * user.txt 획득
  * todo.txt
    *   Stuff to-do:

        1. Keep checking the issues list.
        2. Fix the router config.

        Done:

        1. Restricted access for guest user.
  * vmware 툴 존재
  * Hazard, Public, Administrator 폴더 존재 but 액세스 권한 없음
* 쓸모있는 경로 : /usr/share/doc/python3-impoacket/examples
*
