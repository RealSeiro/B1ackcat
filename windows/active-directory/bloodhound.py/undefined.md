# 예시

{% code lineNumbers="true" fullWidth="false" %}
```bash
┌──(b1ackcat㉿kali)-[~/Downloads/HTB_LAB/Support]
└─$ bloodhound-python -c ALL -u ldap -p 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' -d support.htb -ns 10.10.11.174 
INFO: Found AD domain: support.htb
INFO: Getting TGT for user
INFO: Connecting to LDAP server: dc.support.htb
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 4 computers
INFO: Connecting to LDAP server: dc.support.htb
INFO: Found 21 users
INFO: Found 53 groups
INFO: Found 2 gpos
INFO: Found 1 ous
INFO: Found 19 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: FAKE01.support.htb
INFO: Querying computer: FAKE-COMP01.support.htb
INFO: Querying computer: Management.support.htb
INFO: Querying computer: dc.support.htb
WARNING: Could not resolve: FAKE-COMP01.support.htb: The DNS query name does not exist: FAKE-COMP01.support.htb.
WARNING: Could not resolve: FAKE01.support.htb: The DNS query name does not exist: FAKE01.support.htb.
INFO: Done in 00M 31S
```
{% endcode %}

```bash
```
