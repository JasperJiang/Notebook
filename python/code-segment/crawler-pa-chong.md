# Crawler 爬虫

```python
# coding: UTF-8
import ur llib2
from bs4 import BeautifulSoup

url_or = 'http://www.zmz2017.com'
req = urllib2.Request('http://www.zmz2017.com')


resp = urllib2.urlopen(req)
soup = BeautifulSoup(resp.read(), 'lxml')

top24_soup = soup.select('.top24 ul li a')

top24 = []

for x in range(len(top24_soup)):
    top24.append({'name':top24_soup[x].get_text()})

for x in range(len(top24_soup)):
    top24[x]['href'] = url_or+top24_soup[x]['href']
```



```bash
import requests
from bs4 import BeautifulSoup

url_or = "http://www.dy2018.com/html/gndy/dyzz/index.html"

req = requests.get(url_or)
req.encoding = 'gb18030'

soup = BeautifulSoup(req.text, 'html.parser')

# items = soup.find_all('.co_content8 ul a')
items = soup.find_all("table", class_='tbspan')

print items
```

