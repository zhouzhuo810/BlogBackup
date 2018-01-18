---
title: Python-修改hosts文件
date: 2018-01-18 17:32:53
tags:
	- Python 
categories: Python
---

### 准备
- Window 7/8/10
- Node.js环境
- Python环境
- requests插件
```
pip install requests
```
- beautifulsoup4插件
```
pip install beautifulsoup4
```

<!-- more -->

### 主要代码

```python
s = """
github.com
assets-cdn.github.com
avatars0.githubusercontent.com
avatars1.githubusercontent.com
github-cloud.s3.amazonaws.com
documentcloud.github.com 
help.github.com
nodeload.github.com
raw.github.com
status.github.com
training.github.com
github.io
"""
import requests
from bs4 import BeautifulSoup
  
ans = []
for i in s.split():
    url = "http://ip.chinaz.com/" + i.strip()
    resp = requests.get(url)
    soup = BeautifulSoup(resp.text)
    x = soup.find(class_="IcpMain02")
    x = x.find_all("span", class_="Whwtdhalf")
    x = "%s %s" % (x[5].string.strip(), i.strip())
    print(x)
    ans.append(x)
  
hosts = r"C:\Windows\System32\drivers\etc\hosts"
with open(hosts, "r") as f:
    content = [i for i in f.readlines() if i.startswith("#")]
    content.extend(ans)
with open(hosts, "w") as f:
    f.write("\n".join(content))
```

### 刷新DNS缓存

```
ipconfig /flushdns
```