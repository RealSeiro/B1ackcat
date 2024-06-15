# MSFconsole

### MSF콘솔 명령



| **명령**                                          | **설명**                                                                                |
| ----------------------------------------------- | ------------------------------------------------------------------------------------- |
| `show exploits`                                 | 프레임워크 내의 모든 익스플로잇을 표시합니다.                                                             |
| `show payloads`                                 | 프레임워크 내의 모든 페이로드를 표시합니다.                                                              |
| `show auxiliary`                                | 프레임워크 내의 모든 보조 모듈을 표시합니다.                                                             |
| `search <name>`                                 | 프레임워크 내에서 익스플로잇이나 모듈을 검색합니다.                                                          |
| `info`                                          | 특정 익스플로잇이나 모듈에 대한 정보를 로드합니다.                                                          |
| `use <name>`                                    | 익스플로잇이나 모듈을 로드합니다(예: windows/smb/psexec 사용).                                          |
| `use <number>`                                  | 검색 명령 다음에 표시되는 인덱스 번호를 사용하여 익스플로잇을 로드합니다.                                             |
| `LHOST`                                         | 대상이 연결할 수 있는 로컬 호스트의 IP 주소, 로컬 네트워크에 있지 않은 경우 공용 IP 주소인 경우가 많습니다. 일반적으로 리버스 쉘에 사용됩니다. |
| `RHOST`                                         | 원격 호스트 또는 대상. set 함수 특정 값(예: LHOST 또는 RHOST)을 설정합니다.                                  |
| `setg <function>`                               | 특정 값을 전역적으로 설정합니다(예: LHOST 또는 RHOST).                                                 |
| `show options`                                  | 모듈이나 익스플로잇에 사용할 수 있는 옵션을 표시합니다.                                                       |
| `show targets`                                  | 익스플로잇이 지원하는 플랫폼을 표시합니다.                                                               |
| `set target <number>`                           | OS 및 서비스 팩을 알고 있는 경우 특정 대상 인덱스를 지정하십시오.                                               |
| `set payload <payload>`                         | 사용할 페이로드를 지정합니다.                                                                      |
| `set payload <number>`                          | show payloads 명령 다음에 사용할 페이로드 인덱스 번호를 지정합니다.                                          |
| `show advanced`                                 | 고급 옵션을 표시합니다.                                                                         |
| `set autorunscript migrate -f`                  | 익스플로잇 완료 시 자동으로 별도의 프로세스로 마이그레이션합니다.                                                  |
| `check`                                         | 대상이 공격에 취약한지 확인합니다.                                                                   |
| `exploit`                                       | 모듈을 실행하거나 대상을 악용하여 공격합니다.                                                             |
| `exploit -j`                                    | 작업 컨텍스트에서 익스플로잇을 실행합니다. (이렇게 하면 백그라운드에서 익스플로잇이 실행됩니다.)                                |
| `exploit -z`                                    | 악용에 성공한 후에는 세션과 상호 작용하지 마세요.                                                          |
| `exploit -e <encoder>`                          | 사용할 페이로드 인코더를 지정합니다(예: explore –e shikata\_ga\_nai).                                  |
| `exploit -h`                                    | Exploit 명령에 대한 도움말을 표시합니다.                                                            |
| `sessions -l`                                   | 사용 가능한 세션을 나열합니다(여러 셸을 처리할 때 사용됨).                                                    |
| `sessions -l -v`                                | 사용 가능한 모든 세션을 나열하고 시스템을 악용할 때 사용된 취약점과 같은 자세한 필드를 표시합니다.                              |
| `sessions -s <script>`                          | 모든 Meterpreter 라이브 세션에서 특정 Meterpreter 스크립트를 실행합니다.                                   |
| `sessions -K`                                   | 모든 라이브 세션을 종료합니다.                                                                     |
| `sessions -c <cmd>`                             | 모든 라이브 Meterpreter 세션에서 명령을 실행합니다.                                                    |
| `sessions -u <sessionID>`                       | 일반 Win32 셸을 Meterpreter 콘솔로 업그레이드합니다.                                                 |
| `db_create <name>`                              | 데이터베이스 기반 공격에 사용할 데이터베이스를 생성합니다(예: db\_create autopwn).                               |
| `db_connect <name>`                             | 구동 공격을 위한 데이터베이스를 생성하고 연결합니다(예: db\_connect autopwn).                                 |
| `db_nmap`                                       | Nmap을 사용하고 결과를 데이터베이스에 저장하세요. ( –sT –v –P0과 같은 일반 Nmap 구문이 지원됩니다.)                    |
| `db_destroy`                                    | 현재 데이터베이스를 삭제합니다.                                                                     |
| `db_destroy <user:password@host:port/database>` | 고급 옵션을 사용하여 데이터베이스를 삭제합니다.                                                            |
|                                                 |                                                                                       |

***

### 미터프리터 명령



| **명령**                                                | **설명**                                                                  |
| ----------------------------------------------------- | ----------------------------------------------------------------------- |
| `help`                                                | Meterpreter 사용 도움말을 엽니다.                                                |
| `run <scriptname>`                                    | Meterpreter 기반 스크립트를 실행합니다. 전체 목록을 보려면 scripts/meterpreter 디렉토리를 확인하세요. |
| `sysinfo`                                             | 손상된 대상에 대한 시스템 정보를 표시합니다.                                               |
| `ls`                                                  | 대상의 파일과 폴더를 나열합니다.                                                      |
| `use priv`                                            | 확장된 Meterpreter 라이브러리에 대한 권한 확장을 로드합니다.                                 |
| `ps`                                                  | 실행 중인 모든 프로세스와 각 프로세스와 연결된 계정을 표시합니다.                                   |
| `migrate <proc. id>`                                  | 특정 프로세스 ID로 마이그레이션합니다(PID는 ps 명령에서 얻은 대상 프로세스 ID입니다).                   |
| `use incognito`                                       | 시크릿 기능을 로드합니다. (대상 컴퓨터에서 토큰 도용 및 가장에 사용됩니다.)                            |
| `list_tokens -u`                                      | 사용자별로 대상에서 사용 가능한 토큰을 나열합니다.                                            |
| `list_tokens -g`                                      | 그룹별로 대상에서 사용 가능한 토큰을 나열합니다.                                             |
| `impersonate_token <DOMAIN_NAMEUSERNAME>`             | 대상에서 사용 가능한 토큰을 가장합니다.                                                  |
| `steal_token <proc. id>`                              | 특정 프로세스에 사용 가능한 토큰을 훔치고 해당 토큰을 가장합니다.                                   |
| `drop_token`                                          | 현재 토큰 가장을 중지합니다.                                                        |
| `getsystem`                                           | 여러 공격 벡터를 통해 시스템 수준 액세스 권한을 높이려고 시도합니다.                                 |
| `shell`                                               | 사용 가능한 모든 토큰이 포함된 대화형 셸에 참여하세요.                                         |
| `execute -f <cmd.exe> -i`                             | cmd.exe를 실행하고 상호작용합니다.                                                  |
| `execute -f <cmd.exe> -i -t`                          | 사용 가능한 모든 토큰을 사용하여 cmd.exe를 실행합니다.                                      |
| `execute -f <cmd.exe> -i -H -t`                       | 사용 가능한 모든 토큰을 사용하여 cmd.exe를 실행하고 숨겨진 프로세스로 만듭니다.                        |
| `rev2self`                                            | 대상을 손상시키는 데 사용한 원래 사용자로 되돌립니다.                                          |
| `reg <command>`                                       | 대상 레지스트리에서 상호 작용, 생성, 삭제, 쿼리, 설정 등을 수행합니다.                              |
| `setdesktop <number>`                                 | 로그인한 사람에 따라 다른 화면으로 전환됩니다.                                              |
| `screenshot`                                          | 대상 화면의 스크린샷을 찍습니다.                                                      |
| `upload <filename>`                                   | 대상에 파일을 업로드합니다.                                                         |
| `download <filename>`                                 | 대상에서 파일을 다운로드합니다.                                                       |
| `keyscan_start`                                       | 원격 대상에서 키 입력 스니핑을 시작합니다.                                                |
| `keyscan_dump`                                        | 대상에서 캡처된 원격 키를 덤프합니다.                                                   |
| `keyscan_stop`                                        | 원격 대상에서 키 입력 스니핑을 중지합니다.                                                |
| `getprivs`                                            | 대상에 대해 가능한 한 많은 권한을 얻으십시오.                                              |
| `uictl enable <keyboard/mouse>`                       | 키보드 및/또는 마우스를 제어하십시오.                                                   |
| `background`                                          | 현재 Meterpreter 셸을 백그라운드에서 실행합니다.                                        |
| `hashdump`                                            | 대상의 모든 해시를 덤프합니다. 스니퍼 사용 스니퍼 모듈을 로드합니다.                                 |
| `sniffer_interfaces`                                  | 대상에서 사용 가능한 인터페이스를 나열합니다.                                               |
| `sniffer_dump <interfaceID> pcapname`                 | 원격 대상에 대한 스니핑을 시작합니다.                                                   |
| `sniffer_start <interfaceID> packet-buffer`           | 패킷 버퍼의 특정 범위로 스니핑을 시작합니다.                                               |
| `sniffer_stats <interfaceID>`                         | 스니핑 중인 인터페이스에서 통계 정보를 가져옵니다.                                            |
| `sniffer_stop <interfaceID>`                          | 스니퍼를 중지하세요.                                                             |
| `add_user <username> <password> -h <ip>`              | 원격 대상에 사용자를 추가합니다.                                                      |
| `add_group_user <"Domain Admins"> <username> -h <ip>` | 원격 대상의 도메인 관리자 그룹에 사용자 이름을 추가합니다.                                       |
| `clearev`                                             | 대상 컴퓨터에서 이벤트 로그를 지웁니다.                                                  |
| `timestomp`                                           | 생성 날짜와 같은 파일 속성을 변경합니다(포렌식 방지 조치).                                      |
| `reboot`                                              | 대상 머신을 재부팅합니다.                                                          |
