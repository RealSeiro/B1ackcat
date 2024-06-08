# Page 1

```python
import requests
import re
import hashlib

url = "http://94.237.54.176:52585/"

session = requests.Session()

#Step 1 - Get String

r = session.get(url)

html = r.text

match = re.search("[a-zA-Z0-9]{20}", html) #조건 

#Step 2 - encrypt string with md5

string = match.group() #group : 매칭된 문자열을 한번에 변환

hash = hashlib.md5(string.encode("utf")).hexdigest()

#Step 3 - Post hash to web
p = session.post(url, data={"hash":hash})

print(p.text)
```
