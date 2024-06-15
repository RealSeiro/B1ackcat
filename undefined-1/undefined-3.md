# 웹 프록시

## 웹 프록시 및 평가 사용



[REGEX 일치 및 대체 규칙](https://academy.hackthebox.com/module/110/section/1050)

> 정규식 일치 : True

```
^User-Agent.*$
```

> 바꾸다

```
User-Agent: HackTheBox Agent 1.0
```

[MetaSploit - 보조/스캐너/http/](https://academy.hackthebox.com/module/110/section/1053)

> 웹 프록시를 통해 MSFConsole 요청을 보내 원격 대상에 대한 트래픽 전송 및 요청을 확인합니다. 예를 들어 파일 넣기를 수행합니다 `msf test file`.

```
sudo msfconsole

use auxiliary/scanner/http/http_put

set PROXIES HTTP:127.0.0.1:8080
set RHOST 46.101.95.166
set RPORT 30118
```

[침입자 페이로드 및 단어 목록 - Fuzzer](https://academy.hackthebox.com/module/110/section/1056)

> 다음은 SecLists의 빠르고 짧은 공통 단어 목록입니다.

```
/usr/share/seclists/Discovery/Web-Content/common.txt

/usr/share/seclists/Usernames/top-usernames-shortlist.txt

/usr/share/seclists/Fuzzing/alphanum-case.txt
```

> Burp Intruder 페이로드는 위의 페이로드로 공격합니다.
