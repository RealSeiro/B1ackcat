# Bloodhound.py

**BloodHound.py 옵션**

&#x20; BloodHound.py 옵션

{% code lineNumbers="true" %}
```
realblackcat@htb[/htb]$ bloodhound-python -h

usage: bloodhound-python [-h] [-c COLLECTIONMETHOD] [-u USERNAME]
                         [-p PASSWORD] [-k] [--hashes HASHES] [-ns NAMESERVER]
                         [--dns-tcp] [--dns-timeout DNS_TIMEOUT] [-d DOMAIN]
                         [-dc HOST] [-gc HOST] [-w WORKERS] [-v]
                         [--disable-pooling] [--disable-autogc] [--zip]

Python based ingestor for BloodHound
For help or reporting issues, visit https://github.com/Fox-IT/BloodHound.py

optional arguments:
  -h, --help            show this help message and exit
  -c COLLECTIONMETHOD, --collectionmethod COLLECTIONMETHOD
                        Which information to collect. Supported: Group,
                        LocalAdmin, Session, Trusts, Default (all previous),
                        DCOnly (no computer connections), DCOM, RDP,PSRemote,
                        LoggedOn, ObjectProps, ACL, All (all except LoggedOn).
                        You can specify more than one by separating them with
                        a comma. (default: Default)
  -u USERNAME, --username USERNAME
                        Username. Format: username[@domain]; If the domain is
                        unspecified, the current domain is used.
  -p PASSWORD, --password PASSWORD
                        Password

  <SNIP>
```
{% endcode %}

* 보시다시피 이 도구는 `-c` 또는 `--collectionmethod` 플래그를 사용하여 다양한 수집 방법을 허용
  * 사용자 세션, 사용자 및 그룹, 개체 속성, ACLS와 같은 특정 데이터를 검색하거나 `ALL`선택하여 가능한 한 많은 데이터를 수집할 수 있습니다. 이 방법으로 실행해 보겠습니다.

참고: Kerberos 인증을 사용하려면 호스트가 도메인 FQDN을 확인해야 합니다. 즉, 호스트가 Kerberos가 작동하려면 DNS 이름 KDC를 확인해야 하므로 `--nameserver` 옵션으로는 Kerberos 인증에 충분하지 않습니다. Kerberos 인증을 사용하려면 DNS 서버를 대상 컴퓨터로 설정하거나 호스트 파일에서 DNS 항목을 구성해야 합니다.



**etc/hosts 파일 설정하기**

```bash
realblackcat@htb[/htb]$ echo -e "\n10.129.204.207 dc01.inlanefreight.htb dc01 inlanefreight inlanefreight.htb" | sudo tee -a /etc/hosts

10.129.204.207 dc01.inlanefreight.htb dc01 inlanefreight inlanefreight.htb
```

**BloodHound.py 실행**

&#x20; BloodHound.py 실행

{% code lineNumbers="true" %}
```bash
realblackcat@htb[/htb]$ bloodhound-python -d inlanefreight.htb -c DCOnly -u htb-student -p HTBRocks! -ns 10.129.204.207 --kerberos
INFO: Found AD domain: inlanefreight.htb
INFO: Getting TGT for user
INFO: Connecting to LDAP server: dc01.inlanefreight.htb
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Connecting to LDAP server: dc01.inlanefreight.htb
INFO: Found 6 users
INFO: Found 52 groups
INFO: Found 2 gpos
INFO: Found 1 ous
INFO: Found 19 containers
INFO: Found 3 computers
INFO: Found 0 trusts
INFO: Done in 00M 11S

```
{% endcode %}

위의 명령은 사용자 `forend로` Bloodhound.py를 실행했습니다. 네임서버는 `-ns` 플래그를 사용하여 도메인 컨트롤러로, 도메인은 `-d` 플래그를 사용하여 INLANEFREIGHt.LOCAL로 지정했습니다. `-c all` 플래그는 도구에 모든 검사를 실행하도록 지시했습니다. 스크립트가 완료되면 현재 작업 디렉터리에 \<date\_object.json> 형식의 출력 파일을 볼 수 있습니다.

**결과 보기**

&#x20; 결과 보기

```
realblackcat@htb[/htb]$ ls

20220307163102_computers.json  20220307163102_domains.json  20220307163102_groups.json  20220307163102_users.json  
```

**블러드하운드 GUI에 Zip 파일 업로드**

그런 다음 `sudo neo4j start를` 입력하여 [neo4j](https://neo4j.com/) 서비스를 시작하고, 데이터를 로드할 데이터베이스를 실행하고 Cypher 쿼리를 실행할 수 있습니다.

다음으로, `freerdp를` 사용하여 로그인한 Linux 공격 호스트에서 `bloodhound를` 입력하여 BloodHound GUI 애플리케이션을 시작하고 데이터를 업로드할 수 있습니다. 자격 증명은 Linux 공격 호스트에 미리 입력되어 있지만, 어떤 이유로 자격 증명 프롬프트가 표시되는 경우 이를 사용하세요:

* `사용자 == neo4j` / `패스 == HTB_@cademy_stdnt!`

위의 모든 작업이 완료되면 BloodHound GUI 도구에 빈 슬레이트가 로드되어 있어야 합니다. 이제 데이터를 업로드해야 합니다. 각 JSON 파일을 하나씩 업로드하거나 `zip -r ilfreight_bh.zip *.json과` 같은 명령으로 먼저 압축한 후 Zip 파일을 업로드할 수 있습니다. 창 오른쪽에 있는 `데이터 업로드` 버튼(녹색 화살표)을 클릭하면 됩니다. 파일을 선택할 수 있는 파일 브라우저 창이 나타나면 zip 파일(또는 각 JSON 파일)을 선택하고(빨간색 화살표) `열기를` 누릅니다.

**Zip 파일 업로드**

<figure><img src="https://academy.hackthebox.com/storage/modules/143/bh-injest.png" alt="image"><figcaption></figcaption></figure>

이제 데이터가 로드되었으므로 분석 탭을 사용하여 데이터베이스에 대한 쿼리를 실행할 수 있습니다. 이러한 쿼리는 사용자 지정 [Cypher 쿼리를](https://hausec.com/2019/09/09/bloodhound-cypher-cheatsheet/) 사용하여 결정한 내용에 따라 사용자 지정할 수 있습니다. 여기에 도움이 되는 많은 유용한 치트 시트가 있습니다. 사용자 지정 Cypher 쿼리에 대해서는 이후 섹션에서 자세히 설명하겠습니다. 아래에서 볼 수 있듯이, 창 `왼쪽의` `분석 탭에서` 기본 제공되는 `경로 찾기` 쿼리를 사용할 수 있습니다.

**관계 검색**

<figure><img src="https://academy.hackthebox.com/storage/modules/143/bh-analysis.png" alt="image"><figcaption></figcaption></figure>

위의 맵을 생성하기 위해 선택한 쿼리는 `도메인 관리자로 가는 최단 경로 찾기였습니다`. 이 쿼리는 사용자/그룹/호스트/ACL/GPO 등을 통해 찾은 논리적 경로, 즉 도메인 관리자 권한 또는 이와 동등한 권한으로 에스컬레이션할 수 있는 관계를 제공합니다. 이는 네트워크를 통한 측면 이동을 위한 다음 단계를 계획할 때 매우 유용합니다. 데이터를 업로드한 후 `데이터베이스 정보` 탭에서 `도메인 사용자와` 같은 노드를 검색하고, `노드 정보` 탭 아래의 모든 옵션을 스크롤하고, `분석` 탭 아래에 있는 미리 작성된 쿼리 중 강력하고 도메인 탈취를 위한 다양한 방법을 빠르게 찾을 수 있는 쿼리를 확인해 보세요. 마지막으로, 위에 링크된 Cypher 치트시트에서 흥미로운 쿼리를 몇 가지 선택하여 하단의 `원시 쿼리` 상자에 붙여넣고 Enter 키를 눌러 사용자 지정 Cypher 쿼리를 실험해 보세요. 화면 오른쪽에 있는 톱니바퀴 아이콘을 클릭하고 노드와 에지 표시 방식, 쿼리 디버그 모드 활성화, 다크 모드 활성화 등을 조정하여 `설정` 메뉴를 사용해 볼 수도 있습니다. 이 모듈의 나머지 부분에서는 다양한 방법으로 BloodHound를 사용할 예정이지만, BloodHound 도구에 대한 자세한 내용은 [Active Directory BloodHound](https://academy.hackthebox.com/course/preview/active-directory-bloodhound) 모듈을 확인하세요.

다음 섹션에서는 도메인에 가입된 Windows 호스트에서 SharpHound 수집기를 실행하는 방법을 다루고 BloodHound GUI에서 데이터로 작업하는 몇 가지 예제를 살펴보겠습니다.





