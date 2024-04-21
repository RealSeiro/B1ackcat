# 110 - IMAP/POP3

```bash
curl -k 'imaps://<FQDN/IP>' --user <user>:<password> 	Log in to the IMAPS service using cURL.
openssl s_client -connect <FQDN/IP>:imaps 	Connect to the IMAPS service.
openssl s_client -connect <FQDN/IP>:pop3s 	Connect to the POP3s service.
```

### Telnet 로그인 가능

```bash
telnet 10.10.10.51 110
#RETR 1 : 메일 읽기
```
