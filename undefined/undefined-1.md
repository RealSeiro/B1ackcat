# 정보수집

### 후이즈



| **명령**                       | **설명**              |
| ---------------------------- | ------------------- |
| `export TARGET="domain.tld"` | 환경 변수에 대상을 할당합니다.   |
| `whois $TARGET`              | 대상에 대한 WHOIS 조회입니다. |

***

### DNS 열거



| **명령**                             | **설명**                         |
| ---------------------------------- | ------------------------------ |
| `nslookup $TARGET`                 | `A`대상 도메인에 대한 레코드를 식별합니다 .     |
| `nslookup -query=A $TARGET`        | `A`대상 도메인에 대한 레코드를 식별합니다 .     |
| `dig $TARGET @<nameserver/IP>`     | `A`대상 도메인에 대한 레코드를 식별합니다 .     |
| `dig a $TARGET @<nameserver/IP>`   | `A`대상 도메인에 대한 레코드를 식별합니다 .     |
| `nslookup -query=PTR <IP>`         | `PTR`대상 IP 주소에 대한 레코드를 식별합니다 . |
| `dig -x <IP> @<nameserver/IP>`     | `PTR`대상 IP 주소에 대한 레코드를 식별합니다 . |
| `nslookup -query=ANY $TARGET`      | `ANY`대상 도메인에 대한 레코드를 식별합니다 .   |
| `dig any $TARGET @<nameserver/IP>` | `ANY`대상 도메인에 대한 레코드를 식별합니다 .   |
| `nslookup -query=TXT $TARGET`      | `TXT`대상 도메인에 대한 레코드를 식별합니다 .   |
| `dig txt $TARGET @<nameserver/IP>` | `TXT`대상 도메인에 대한 레코드를 식별합니다 .   |
| `nslookup -query=MX $TARGET`       | `MX`대상 도메인에 대한 레코드를 식별합니다 .    |
| `dig mx $TARGET @<nameserver/IP>`  | `MX`대상 도메인에 대한 레코드를 식별합니다 .    |

***

### 수동 하위 도메인 열거



| **자원/명령**                                                                                                          | **설명**                                                                             |
| ------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------- |
| `VirusTotal`                                                                                                       | [https://www.virustotal.com/gui/home/url](https://www.virustotal.com/gui/home/url) |
| `Censys`                                                                                                           | [https://censys.io/](https://censys.io/)                                           |
| `Crt.sh`                                                                                                           | [https://crt.sh/](https://crt.sh/)                                                 |
| `curl -s https://sonar.omnisint.io/subdomains/{domain} \| jq -r '.[]' \| sort -u`                                  | 특정 도메인의 모든 하위 도메인.                                                                 |
| `curl -s https://sonar.omnisint.io/tlds/{domain} \| jq -r '.[]' \| sort -u`                                        | 특정 도메인에 대해 발견된 모든 TLD.                                                             |
| `curl -s https://sonar.omnisint.io/all/{domain} \| jq -r '.[]' \| sort -u`                                         | 특정 도메인에 대한 모든 TLD의 모든 결과입니다.                                                       |
| `curl -s https://sonar.omnisint.io/reverse/{ip} \| jq -r '.[]' \| sort -u`                                         | IP 주소에 대한 역방향 DNS 조회.                                                              |
| `curl -s https://sonar.omnisint.io/reverse/{ip}/{mask} \| jq -r '.[]' \| sort -u`                                  | CIDR 범위의 역방향 DNS 조회입니다.                                                            |
| `curl -s "https://crt.sh/?q=${TARGET}&output=json" \| jq -r '.[] \| "\(.name_value)\n\(.common_name)"' \| sort -u` | 인증서 투명성.                                                                           |
| `cat sources.txt \| while read source; do theHarvester -d "${TARGET}" -b $source -f "${source}-${TARGET}";done`    | source.txt 목록에 제공된 소스에 대한 하위 도메인 및 기타 정보를 검색합니다.                                   |

**소스.txt**



```
baidu
bufferoverun
crtsh
hackertarget
otx
projecdiscovery
rapiddns
sublist3r
threatcrowd
trello
urlscan
vhost
virustotal
zoomeye
```

***

### 패시브 인프라 식별



| **자원/명령**                                              | **설명**                                                                               |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------ |
| `Netcraft`                                             | [https://www.netcraft.com/](https://www.netcraft.com/)                               |
| `WayBackMachine`                                       | [http://web.archive.org/](http://web.archive.org/)                                   |
| `WayBackURLs`                                          | [https://github.com/tomnomnom/waybackurls](https://github.com/tomnomnom/waybackurls) |
| `waybackurls -dates https://$TARGET > waybackurls.txt` | 도메인을 획득한 날짜를 사용하여 도메인에서 URL을 크롤링합니다.                                                 |

***

### 활성 인프라 식별



| **자원/명령**                                                                 | **설명**                                                                               |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| `curl -I "http://${TARGET}"`                                              | 대상 웹서버의 HTTP 헤더를 표시합니다.                                                              |
| `whatweb -a https://www.facebook.com -v`                                  | 기술 식별.                                                                               |
| `Wappalyzer`                                                              | [https://www.wappalyzer.com/](https://www.wappalyzer.com/)                           |
| `wafw00f -v https://$TARGET`                                              | WAF 핑거프린팅.                                                                           |
| `Aquatone`                                                                | [https://github.com/michenriksen/aquatone](https://github.com/michenriksen/aquatone) |
| `cat subdomain.list \| aquatone -out ./aquatone -screenshot-timeout 1000` | subdomain.list에 있는 모든 하위 도메인의 스크린샷을 만듭니다.                                            |

***

### 활성 하위 도메인 열거



| **자원/명령**                                                                                                  | **설명**                                                                                   |
| ---------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| `HackerTarget`                                                                                             | [https://hackertarget.com/zone-transfer/](https://hackertarget.com/zone-transfer/)       |
| `SecLists`                                                                                                 | [https://github.com/danielmiessler/SecLists](https://github.com/danielmiessler/SecLists) |
| `nslookup -type=any -query=AXFR $TARGET nameserver.target.domain`                                          | 대상 도메인 및 해당 이름 서버에 대해 Nslookup을 사용하여 영역 전송.                                              |
| `gobuster dns -q -r "${NS}" -d "${TARGET}" -w "${WORDLIST}" -p ./patterns.txt -o "gobuster_${TARGET}.txt"` | 무차별 하위 도메인.                                                                              |

***

### 가상 호스트



| **자원/명령**                                                                                                                                                                                  | **설명**                               |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------ |
| `curl -s http://192.168.10.10 -H "Host: randomtarget.com"`                                                                                                                                 | 특정 도메인을 요청하기 위해 HOST HTTP 헤더를 변경합니다. |
| `cat ./vhosts.list \| while read vhost;do echo "\n********\nFUZZING: ${vhost}\n********";curl -s -I http://<IP address> -H "HOST: ${vhost}.target.domain" \| grep "Content-Length: ";done` | 대상 도메인의 가능한 가상 호스트에 대한 무차별 대입.       |
| `ffuf -w ./vhosts -u http://<IP address> -H "HOST: FUZZ.target.domain" -fs 612`                                                                                                            | .`ffuf`​                             |

***

### 크롤링



| **자원/명령**                                                                                                                                            | **설명**                                               |
| ---------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| `ZAP`                                                                                                                                                | [https://www.zaproxy.org/](https://www.zaproxy.org/) |
| `ffuf -recursion -recursion-depth 1 -u http://192.168.10.10/FUZZ -w /opt/useful/SecLists/Discovery/Web-Content/raft-small-directories-lowercase.txt` | 웹사이트 탐색으로 찾을 수 없는 파일과 폴더를 발견합니다.                     |
| `ffuf -w ./folders.txt:FOLDERS,./wordlist.txt:WORDLIST,./extensions.txt:EXTENSIONS -u http://www.target.domain/FOLDERS/WORDLISTEXTENSIONS`           | 대상 웹 서버에 대한 돌연변이 무차별 공격.                             |
