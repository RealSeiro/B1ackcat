# GPP 비밀번호

새 GPP(그룹 정책 환경설정)가 생성될 때마다 해당 구성 데이터와 함께 해당 GPP와 관련된 비밀번호를 포함한 xml 파일이 SYSVOL 공유에 생성됩니다. 보안을 위해 Microsoft AES는 암호를 `cpassword로` 저장하기 전에 암호를 암호화합니다. 하지만 Microsoft는 MSDN에 [키를 공개](https://msdn.microsoft.com/en-us/library/2c15cbf0-f086-4c74-8b70-1f2fa45dd4be.aspx) 했습니다!

Microsoft는 2014년에 관리자가 GPP에 비밀번호를 입력하지 못하도록 하는 패치를 발표했습니다. 하지만 이 패치는 이미 존재하는 이러한 취약한 비밀번호에 대해서는 아무런 조치를 취하지 않았으며, 제가 알기로는 2018년에도 펜테스터들이 여전히 이러한 비밀번호를 정기적으로 발견하고 있습니다. 자세한 내용은 이 [AD 보안 게시물을](https://adsecurity.org/?p=2288) 확인하세요.

#### GPP 비밀번호 해독하기 <a href="#decrypting-gpp-password" id="decrypting-gpp-password"></a>

키를 알고 있기 때문에 비밀번호를 해독할 수 있습니다. Kali에는 이를 수행하는 `gpp-decrypt라는` 도구가 있습니다:

```bash
gpp-decrypt edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ
GPPstillStandingStrong2k18 #해독된 비밀번호 
```
