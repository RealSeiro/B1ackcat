# 80 - http

```bash
// 기술스택 보는 법 
curl --insecure -I https://streamio.htb/
HTTP/2 200 
cache-control: no-store, no-cache, must-revalidate
pragma: no-cache
content-length: 0
content-type: text/html; charset=UTF-8
expires: Thu, 19 Nov 1981 08:52:00 GMT
server: Microsoft-IIS/10.0
x-powered-by: PHP/7.2.26
set-cookie: PHPSESSID=v1gcilv695ahe0bb8o2l8fde0c; path=/
x-powered-by: ASP.NET
date: Fri, 22 Dec 2023 10:46:23 GMT

```

## 웹사이트 디렉토리 열거&#x20;

* gobuster
* Feroxbuster
* wfuzz

```bash
//wfuzz
wfuzz -c -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt --hh 158607 http://bart.htb/FUZZ
#-c : 응답 보여주기, -hh : 필터링
```

## 무차별 대입

### 웹사이트에 있는 문장들로 단어장 만들기

```bash
cewl -w cewl-forum.txt -e -a http://forum.bart.htb
#-e : 이메일도 단어목록에 포함  
```

r = requests.get('http://internal-01.bart.htb/log/log.php?filename=phpinfo.php\&username=harvey', proxies=proxies, headers=headers)
