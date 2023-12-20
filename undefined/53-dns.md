# 53 - DNS

DNS(도메인 이름 시스템)는 인터넷의 전화번호부입니다. 사람들은 nytimes.com 또는 espn.com과 같은 도메인 이름을 통해 온라인 정보에 액세스합니다. 웹 브라우저는 인터넷 프로토콜(IP) 주소를 통해 상호 작용합니다. DNS는 브라우저에서 인터넷 리소스를 로드할 수 있도록 도메인 이름을 [IP 주소로](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/)변환합니다

```bash
dig axfr @10.10.10.175 sauna.htb #영역 전송
dig any victim.com @<DNS_IP>
```
