# FFUF 웹 애플리케이션 공격

## Ffuf 웹 애플리케이션 공격



### 빠른 참조 명령



| **명령**                                                                                                                                                                 | **설명**         |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- |
| `ffuf -h`                                                                                                                                                              | ㅋㅋㅋ 도와주세요      |
| `ffuf -ic -c -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ`                                                                                                       | 디렉토리 퍼징        |
| `ffuf -ic -c -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/indexFUZZ`                                                                                                  | 확장 퍼징          |
| `ffuf -ic -c -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/blog/FUZZ.php`                                                                                              | 페이지 퍼징         |
| `ffuf -ic -c -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v`                                                              | 재귀적 퍼징         |
| `ffuf -ic -c -w wordlist.txt:FUZZ -u https://FUZZ.hackthebox.eu/`                                                                                                      | 하위 도메인 퍼징      |
| `ffuf -ic -c -w wordlist.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb' -fs xxx`                                                                     | VHost 퍼징       |
| `ffuf -ic -c -w wordlist.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php?FUZZ=key -fs xxx`                                                                   | 매개변수 퍼징 - GET  |
| `ffuf -ic -c -w wordlist.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx` | 매개변수 퍼징 - POST |
| `ffuf -c -w ids.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx`           | 가치 퍼징          |

### 단어 목록



> 맞춤 단어 목록 - [값 퍼징](https://academy.hackthebox.com/module/54/section/505)

| **명령**                                                                   | **설명**         |
| ------------------------------------------------------------------------ | -------------- |
| `/usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt` | 디렉토리/페이지 단어 목록 |
| `/usr/share/seclists/Discovery/Web-Content/web-extensions.txt`           | 확장 단어 목록       |
| `/usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt`      | 도메인 단어 목록      |
| `/usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt`     | 매개변수 단어 목록     |

### 기타



| **명령**                                                                                                                        | **설명**                                                             |
| ----------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| `sudo sh -c 'echo "SERVER_IP academy.htb" >> /etc/hosts'`                                                                     | 가상 호스트 라우팅 vHost 이름 확인 헤더에 호스팅된 동일한 IP의 다른 웹사이트에 DNS 항목 지원을 추가합니다. |
| `for i in $(seq 1 1000); do echo $i >> ids.txt; done`                                                                         | 시퀀스 단어 목록 만들기                                                      |
| `curl http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=key' -H 'Content-Type: application/x-www-form-urlencoded'` | POST를 사용한 컬                                                        |

### FFUFING 세부 사항



[FFUF는 ffuf를 사용하여 웹 서버에서 호스팅되는 모든 파일/폴더를 열거합니다.](https://codingo.io/tools/ffuf/bounty/2020/09/17/everything-you-need-to-know-about-ffuf.html#fuzzing-multiple-locations)

#### 견본



1. 루트 디렉터리`ffuf -c -w 9-big.txt -u http://easy.box/FUZZ`
2. 확장자가 있는 루트`ffuf -c -w 9-big.txt -u http://easy.box/FUZZ -e .git,.txt,.json,.php,.html,.bak,.old,.sql,.zip,.conf,.cfg,.asp,.aspx,.cs`
3. 폴더 아래의 하위 웹 폴더`ffuf -c -w 9-big.txt -u http://eezy.box/secret/FUZZ`
4. 확장자가 있는 하위 웹 폴더`ffuf -c -w 9-big.txt -u http://eezy.box/secret/FUZZ -e .git,.txt,.json,.php,.html,.bak,.old,.sql,.zip,.conf,.cfg,.js`
5. vHost 퍼지 도메인`ffuf -c -w 9-big.txt -H "Host: FUZZ.easy.box/" -u http://easy.box/`
6. 하위 도메인 루트 ^^ 발견된 하위 도메인에 대해 1단계를 반복하세요 ^^`ffuf -c -w 9-big.txt -u http://sub.easy.box/FUZZ`
7. 보고`ffuf -c -w common.txt -u http://oscp.sec:8080/FUZZ -o ffuf_report.html -of html`

#### 루트 웹사이트



```
ffuf -c -w /usr/share/seclists/Discovery/Web-Content/big.txt -u http://vulnnet.htb/FUZZ
```

```
ffuf -c -c -w /usr/share/seclists/Discovery/Web-Content/big.txt -u http://spectra.htb/FUZZ
```

```
ffuf -c -w ~/Downloads/wordlists/big.txt -u http://lordoftheroot.box:1337/FUZZ
```

#### ROOT 웹사이트 확장



```
ffuf -c -w typo3_custom.txt -u http://maintest.enterprize.htb/FUZZ -e .old -fc 301 | grep "\.old"
```

```
ffuf -c -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://vulnnet.htb/FUZZ -e .txt,.json,.php,.html,.bak,.old,.sql,.zip,.zz -fc 403
```

```
ffuf -c -c -w ~/Downloads/wordlists/big.txt -u http://lordoftheroot.box:1337/FUZZ -e .git,.txt,.json,.php,.html,.bak,.old,.sql,.zip,.conf,.cfg,.go
```

#### 하위 도메인



> [하위 도메인 퍼징](https://academy.hackthebox.com/module/54/section/488)

> \-fw 응답 단어의 양을 기준으로 필터링합니다. 쉼표로 구분된 단어 수 및 범위 목록 -H Header `"Name: Value"`, 콜론으로 구분. 여러 -H 플래그가 허용됩니다. -fc HTTP 응답 코드 400을 반환하는 잘못된 매개변수 값. 응답 코드 400 필터링 - 잘못된 요청

```
ffuf -c -ic -w subdomains-top1million-5000.txt -u http://FUZZ.academy.htb:12345/ -fc 403
```

#### vHost 도메인



> vHost 퍼징 [HackTheBox Academy - vHost 퍼즈](https://academy.hackthebox.com/module/144/section/1257)\
> 단어 목록 - /usr/share/seclists/Discovery/DNS/namelist.txt

```
ffuf -c -w ./vhosts -u http://192.168.10.10 -H "HOST: FUZZ.randomtarget.com" -fs 612
```

```
ffuf -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.academy.htb" -u http://academy.htb:54542/ -fs 85
```

```
ffuf -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.koikoi.oscp/" -u http://koikoi.oscp/
```

```
ffuf -u http://trick.htb -c -w 0-common-with-mylist.txt -H 'Host: preprod-FUZZ.trick.htb' -fw 1697
```

```
ffuf -c -w /usr/share/seclists/Discovery/Web-Content/big.txt -u http://broadcast.vulnnet.htb/FUZZ -fc 401
```

```
ffuf -u http://sneakycorp.htb -H 'Host: FUZZ.sneakycorp.htb' -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -fw 6
```

```
ffuf -u http://horizontall.htb -H 'Host: FUZZ.forge.htb' -c -w ~/Downloads/wordlists/0-common-with-mylist.txt
```

#### 확장



```
ffuf -c -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://broadcast.vulnnet.htb/FUZZ -e .txt,.json,.php,.html,.bak,.old,.sql,.zip,.zz -fc 403
```

> 루프를 수행하여 발견된 여러 하위 도메인에서 허용된 확장에 대한 퍼지 `for`.

```
for sub in archive test faculty; do ffuf -c -ic -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://$sub.academy.htb:57089/indexFUZZ; done
```

> `for`세 가지 가능한 확장자가 나열된 가능한 파일 이름을 검색하기 위해 루프를 사용하여 여러 하위 도메인을 검색합니다 `.php,.phps,.php7`.

```
for sub in archive test faculty; do ffuf -w /usr/share/seclists/Discovery/Web-Content/raft-large-words-lowercase.txt:FUZZ -u http://$sub.academy.htb:57089/FUZZ -recursion -recursion-depth 1 -e .php,.phps,.php7 -v -t 200 -fs 287 -ic; done
```

#### 알려진 파일 + 확장자



```
ffuf -c -v -c -w ~/Downloads/htb/quick-extensions1.txt -u http://team.htb/scripts/script.FUZZ
```

#### 프록시를 통한 FFUF



```
ffuf -c -c -w /root/Downloads/wordlists/webfuzz_less.txt -u http://pinkyspalace.box:8080/FUZZ -x http://pinkyspalace.box:31337
```

```
ffuf -c -c -w /root/Downloads/wordlists/webfuzz_less.txt -u http://pinkyspalace.box:8080/FUZZ -replay-proxy http://127.0.0.1:8080
```

#### API 엔드포인트



> ' 아래 명령에서 슬래시를 사용하여 작은따옴표를 이스케이프 처리하세요! -- LUA 또는 SQL 등에 대한 나머지 API 쿼리 구문을 주석 처리합니다.

```
ffuf -u http://target IP/weather/forecast?city=\'FUZZ-- -c -w /opt/SecLists/Fuzzing/special-chars.txt -mc 200,500 -fw 9
```

#### 매개변수 값



> [매개변수 퍼징 - GET](https://academy.hackthebox.com/module/54/section/490)\
> [매개변수 퍼징 - POST](https://academy.hackthebox.com/module/54/section/508)

```
ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:54542/admin/admin.php?FUZZ=key -fs xxx
```

```
ffuf -c -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:PARAM -c -w values.txt:VAL -u http://flasky.offsec/add?PARAM=VAL -mr "VAL" -c
```

> 우편

```
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```

```
ffuf -w ids.txt:FUZZ -u http://admin.academy.htb:54542/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs 768
```

> 로 키 값을 발견 `73`하고 을 사용하여 POST를 수행했습니다 `CURL`.

```
curl http://admin.academy.htb:54542/admin/admin.php -X POST -d 'id=73' -H 'Content-Type: application/x-www-form-urlencoded'
```

> HTB{p4r4m373r\_fuzz1n6\_15\_k3y!}

#### API 파일 POST 요청



> [ippsec youtube API 열거형](https://youtu.be/yM914q6zS-U) - IPPSEC - Hackthebox - 인터페이스 API 열거형

```
ffuf -u http://prd.m.rendering-api.interface.htb/FUZZ -c -w /opt/SecLists/Discovery/Web-Content/raft-small-words.txt -mc all -fs 0
```

```
ffuf -u http://prd.m.rendering-api.interface.htb/api/FUZZ -c -w /opt/SecLists/Discovery/Web-Content/raft-small-words.txt -mc all -fs 50 -d 'x=x'
```

```
ffuf -request api.txt -request-proto http -c -w /opt/SecLists/Discovery/Web-Content/api/api-seen-in-wild.txt -mc all -fs 36
```

#### FFuF 웹 보고서



```
ffuf -c -c -w /root/Downloads/wordlists/0-common-with-mylist.txt -u http://oscp.sec:8080/FUZZ -o ffuf_report.html -of html
```

```
ffuf -c -c -w common.txt -u http://192.168.x.y:8080/FUZZ -o ffuf_report.html -of html && firefox ffuf_report.html
```

#### 사용자 이름 열거형 정보 유출



> 로그인 [FFUF 사용자 이름 열거](https://tryhackme.com/room/authenticationbypass)\
> 로그온 사이트에서 사용자가 존재하면 메시지와 함께 표시 = 이 사용자 이름을 가진 계정이 이미 존재합니다.

```
ffuf -c -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.139.148/customers/signup -mr "An account with this username already exists"
```

> 유효한 조합 자격 증명 받기

```
ffuf -c -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.139.148/customers/login -fc 200
```

#### 재귀적



```
ffuf -recursion -recursion-depth 1 -u https://admin.academy.htb:54542/FUZZ -c -w /usr/share/seclists/Discovery/Web-Content/raft-small-directories-lowercase.txt
```

```
ffuf -c -v -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt -u http://94.237.55.13:43548/FUZZ -e .php -recursion -recursion-depth 1
```

```
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php
```

### 돕다



```
Fuzz Faster U Fool - v2.0.0-dev

HTTP OPTIONS:
  -H                  Header `"Name: Value"`, separated by colon. Multiple -H flags are accepted.
  -X                  HTTP method to use
  -b                  Cookie data `"NAME1=VALUE1; NAME2=VALUE2"` for copy as curl functionality.
  -d                  POST data
  -http2              Use HTTP2 protocol (default: false)
  -ignore-body        Do not fetch the response content. (default: false)
  -r                  Follow redirects (default: false)
  -recursion          Scan recursively. Only FUZZ keyword is supported, and URL (-u) has to end in it. (default: false)
  -recursion-depth    Maximum recursion depth. (default: 0)
  -recursion-strategy Recursion strategy: "default" for a redirect based, and "greedy" to recurse on all matches (default: default)
  -replay-proxy       Replay matched requests using this proxy.
  -sni                Target TLS SNI, does not support FUZZ keyword
  -timeout            HTTP request timeout in seconds. (default: 10)
  -u                  Target URL
  -x                  Proxy URL (SOCKS5 or HTTP). For example: http://127.0.0.1:8080 or socks5://127.0.0.1:8080

GENERAL OPTIONS:
  -V                  Show version information. (default: false)
  -ac                 Automatically calibrate filtering options (default: false)
  -acc                Custom auto-calibration string. Can be used multiple times. Implies -ac
  -ach                Per host autocalibration (default: false)
  -ack                Autocalibration keyword (default: FUZZ)
  -acs                Autocalibration strategy: "basic" or "advanced" (default: basic)
  -c                  Colorize output. (default: false)
  -config             Load configuration from a file
  -json               JSON output, printing newline-delimited JSON records (default: false)
  -maxtime            Maximum running time in seconds for entire process. (default: 0)
  -maxtime-job        Maximum running time in seconds per job. (default: 0)
  -noninteractive     Disable the interactive console functionality (default: false)
  -p                  Seconds of `delay` between requests, or a range of random delay. For example "0.1" or "0.1-2.0"
  -rate               Rate of requests per second (default: 0)
  -s                  Do not print additional information (silent mode) (default: false)
  -sa                 Stop on all error cases. Implies -sf and -se. (default: false)
  -scraperfile        Custom scraper file path
  -scrapers           Active scraper groups (default: all)
  -se                 Stop on spurious errors (default: false)
  -search             Search for a FFUFHASH payload from ffuf history
  -sf                 Stop when > 95% of responses return 403 Forbidden (default: false)
  -t                  Number of concurrent threads. (default: 40)
  -v                  Verbose output, printing full URL and redirect location (if any) with the results. (default: false)

MATCHER OPTIONS:
  -mc                 Match HTTP status codes, or "all" for everything. (default: 200,204,301,302,307,401,403,405,500)
  -ml                 Match amount of lines in response
  -mmode              Matcher set operator. Either of: and, or (default: or)
  -mr                 Match regexp
  -ms                 Match HTTP response size
  -mt                 Match how many milliseconds to the first response byte, either greater or less than. EG: >100 or <100
  -mw                 Match amount of words in response

FILTER OPTIONS:
  -fc                 Filter HTTP status codes from response. Comma separated list of codes and ranges
  -fl                 Filter by amount of lines in response. Comma separated list of line counts and ranges
  -fmode              Filter set operator. Either of: and, or (default: or)
  -fr                 Filter regexp
  -fs                 Filter HTTP response size. Comma separated list of sizes and ranges
  -ft                 Filter by number of milliseconds to the first response byte, either greater or less than. EG: >100 or <100
  -fw                 Filter by amount of words in response. Comma separated list of word counts and ranges

INPUT OPTIONS:
  -D                  DirSearch wordlist compatibility mode. Used in conjunction with -e flag. (default: false)
  -e                  Comma separated list of extensions. Extends FUZZ keyword.
  -ic                 Ignore wordlist comments (default: false)
  -input-cmd          Command producing the input. --input-num is required when using this input method. Overrides -w.
  -input-num          Number of inputs to test. Used in conjunction with --input-cmd. (default: 100)
  -input-shell        Shell to be used for running command
  -mode               Multi-wordlist operation mode. Available modes: clusterbomb, pitchfork, sniper (default: clusterbomb)
  -request            File containing the raw http request
  -request-proto      Protocol to use along with raw request (default: https)
  -w                  Wordlist file path and (optional) keyword separated by colon. eg. '/path/to/wordlist:KEYWORD'

OUTPUT OPTIONS:
  -debug-log          Write all of the internal logging to the specified file.
  -o                  Write output to file
  -od                 Directory path to store matched results to.
  -of                 Output file format. Available formats: json, ejson, html, md, csv, ecsv (or, 'all' for all formats) (default: json)
  -or                 Don't create the output file if we don't have results (default: false)

EXAMPLE USAGE:
  Fuzz file paths from wordlist.txt, match all responses but filter out those with content-size 42.
  Colored, verbose output.
    ffuf -w wordlist.txt -u https://example.org/FUZZ -mc all -fs 42 -c -v

  Fuzz Host-header, match HTTP 200 responses.
    ffuf -w hosts.txt -u https://example.org/ -H "Host: FUZZ" -mc 200

  Fuzz POST JSON data. Match all responses not containing text "error".
    ffuf -w entries.txt -u https://example.org/ -X POST -H "Content-Type: application/json" \
      -d '{"name": "FUZZ", "anotherkey": "anothervalue"}' -fr "error"

  Fuzz multiple locations. Match only responses reflecting the value of "VAL" keyword. Colored.
    ffuf -w params.txt:PARAM -w values.txt:VAL -u https://example.org/?PARAM=VAL -mr "VAL" -c
```

## 기술 평가 - 웹 퍼징



> [HackTheBox Academy - 기술 평가 - FFUF](https://academy.hackthebox.com/module/54/section/511)\
> [연습을 통한 웹 퍼징](https://charleskvarga.com/post/htb-academy-attacking-web-applications-with-ffuf-walkthrough/)

> 1. `*.academy.htb`위에 표시된 IP에 대해 하위 도메인/가상 호스트 퍼징 스캔을 실행합니다 . 식별할 수 있는 하위 도메인은 모두 무엇입니까?

```
ffuf -w quick-list.txt:FUZZ -u http://FUZZ.academy.htb:PORT/
```

> 2. 페이지 퍼징 스캔을 실행하기 전에 먼저 확장 퍼징 스캔을 실행해야 합니다. 도메인에서 허용되는 다양한 확장자는 무엇입니까?

```
for sub in archive test faculty; do ffuf -c -ic -w quick-list.txt:FUZZ -u http://$sub.academy.htb:57089/indexFUZZ; done
```

> 3. 귀하가 식별하게 될 페이지 중 하나에는 '액세스 권한이 없습니다!'라는 메시지가 표시되어야 합니다. 전체 페이지 URL은 무엇입니까?

```
for sub in archive test faculty; do ffuf -c -ic -w quick-list.txt:FUZZ -u http://$sub.academy.htb:57089/FUZZ -recursion -recursion-depth 1 -e .php,.phps,.php7 -v -t 200 -fs 287 -fc 403; done
```

> 4. 이전 질문의 페이지에서는 페이지에서 허용되는 여러 매개변수를 찾을 수 있어야 합니다. 그들은 무엇인가?

```
ffuf -c -ic -w quick-list.txt:FUZZ -u http://faculty.academy.htb:57089/courses/linux-security.php7 -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs 774
```

> 5. 작업 값으로 식별한 매개변수를 퍼징해 보세요. 그 중 하나는 플래그를 반환해야 합니다. 깃발의 내용은 무엇입니까?

```
ffuf -w parameters.txt:PARAM -w quick-list.txt:VAL -c -ic -u http://faculty.academy.htb:57089/courses/linux-security.php7 -X POST -d 'PARAM=VAL' -H 'Content-Type: application/x-www-form-urlencoded' -fw 223
```

> POST 컬 요청

```
curl http://faculty.academy.htb:57089/courses/linux-security.php7 -X POST -d 'username=harry' -H 'Content-Type: application/x-www-form-urlencoded' | html2text
```

> HTB{w3b\_fuzz1n6\_m4573r}
