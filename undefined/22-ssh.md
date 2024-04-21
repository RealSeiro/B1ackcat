# 22 - SSH

rbash에서 ssh를 연결할때 ssh 연결 명령에 -t bash를 추가할 경우 bash쉘로 연결 가능

```bash
sshpass -p 'P@55W0rd1!2@' ssh mindy@10.10.10.51 -t bash
```

```bash
ssh-audit.py <FQDN/IP> 	Remote security audit against the target SSH service.
ssh <user>@<FQDN/IP> 	Log in to the SSH server using the SSH client.
ssh -i private.key <user>@<FQDN/IP> 	Log in to the SSH server using private key.
ssh <user>@<FQDN/IP> -o PreferredAuthentications=password 	Enforce password-based authentication.
```
