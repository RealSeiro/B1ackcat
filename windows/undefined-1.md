# 다양한 파일 접근

### mdb 파일&#x20;

```bash
//mdb-tools
mdb-export backup.mdb acc_antiback #mpd파일에서 acc_antiback 테이블 추출
mdb-tables backup.mdb #mdb 파일 읽기 
```

### pst(이메일 폴더) 파일

```bash
//readpst
readpst Access\ Control.pst #pst파일을 mbox 형식으로 변환 

mutt -Rf Access\ Control.mbox #mbox파일을 읽기
```

### 숨겨진 파일 살펴보기&#x20;

```powershell
*Evil-WinRM* PS C:\> gci -recurse -force -file PSTranscripts
```
