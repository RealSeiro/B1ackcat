# 4555 - James

### 취약점 존재

```bash
┌──(b1ackcat㉿kali)-[~/Downloads/HTB_LAB]
└─$ searchsploit james
------------------------------------------------- ---------------------------------
 Exploit Title                                   |  Path
------------------------------------------------- ---------------------------------
Apache James Server 2.2 - SMTP Denial of Service | multiple/dos/27915.pl
Apache James Server 2.3.2 - Insecure User Creati | linux/remote/48130.rb
Apache James Server 2.3.2 - Remote Command Execu | linux/remote/35513.py
Apache James Server 2.3.2 - Remote Command Execu | linux/remote/50347.py
WheresJames Webcam Publisher Beta 2.0.0014 - Rem | windows/remote/944.c
------------------------------------------------- ---------------------------------
Shellcodes: No Results
```

### NC 로그인 가능&#x20;

```bash
nc 10.10.10.51 4555 
#기본 비밀번호 root/root
#help : 도움말
#listusers : 사용자 나열
#setpassword : 비밀번호 변경
```
