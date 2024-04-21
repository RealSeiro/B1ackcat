# 53 - DNS

DNS(도메인 이름 시스템)는 인터넷의 전화번호부입니다. 사람들은 nytimes.com 또는 espn.com과 같은 도메인 이름을 통해 온라인 정보에 액세스합니다. 웹 브라우저는 인터넷 프로토콜(IP) 주소를 통해 상호 작용합니다. DNS는 브라우저에서 인터넷 리소스를 로드할 수 있도록 도메인 이름을 [IP 주소로](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/)변환합니다

```bash
dig axfr @10.10.10.175 sauna.htb #영역 전송
dig any victim.com @<DNS_IP>
```

```bash
#DNS 이름을 사용할때 다른 하위 도메인 찾을떄 쓰는 명령어 
wfuzz -u https://streamio.htb -H "Host: FUZZ.streamio.htb" -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt --hh 315  #-H : HTTP 헤더를 설정,  FUZZ는 wfuzz가 대체할 키워드, --hh : HTTP 상태 코드를 필터링하는 옵션, 여기서는 315 상태 코드를 필터링하여 출력   
```

```bash
// DNS
dig ns <domain.tld> @<nameserver> 	NS request to the specific nameserver.
dig any <domain.tld> @<nameserver> 	ANY request to the specific nameserver.
dig axfr <domain.tld> @<nameserver> 	AXFR request to the specific nameserver.
dnsenum --dnsserver <nameserver> --enum -p 0 -s 0 -o found_subdomains.txt -f ~/subdomains.list <domain.tld> 	Subdomain brute forcing.

#enum DNS record
A 	Returns an IPv4 address of the requested domain as a result.
AAAA 	Returns an IPv6 address of the requested domain.
MX 	Returns the responsible mail servers as a result.
NS 	Returns the DNS servers (nameservers) of the domain.
TXT 	This record can contain various information. The all-rounder can be used, e.g., to validate the Google Search Console or validate SSL certificates. In addition, SPF and DMARC entries are set to validate mail traffic and protect it from spam.
CNAME 	This record serves as an alias. If the domain www.hackthebox.eu should point to the same IP, and we create an A record for one and a CNAME record for the other.
PTR 	The PTR record works the other way around (reverse lookup). It converts IP addresses into valid domain names.
SOA 	Provides information about the corresponding DNS zone and email address of the administrative contact.
```
