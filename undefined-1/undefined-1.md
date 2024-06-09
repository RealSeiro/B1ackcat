# 인코딩/디코딩

[https://hashes.com/en/decrypt/hash](https://hashes.com/en/decrypt/hash) - 해쉬로 된 비밀번호 해독 사이트

```bash
#base64 디코딩 하기 
echo clk0bjVldmE= | base64 -d
```

```bash
#hashcat
hashcat --example-hashe | grep -B 2 '\$1\$' 
#\는 메타문자(특수문자)를 사용하기 위해 붙임, -B 2 : 앞2줄에 있는걸 출력
#-A 2 : 뒷 2줄까지 출력
```
