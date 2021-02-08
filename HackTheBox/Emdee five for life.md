![image-20210208202031721](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20210208202031721.png)

You copy the string on the screen, paste to a md5 hash tool online, get the cipher text then submit it, you will always receive this notification:

![image-20210208202343339](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20210208202343339.png)

We need a fast and accuracy way, so I decided to write a python script to automate the action

```python
import requests
import hashlib
import bs4

url = 'http://206.189.121.131:30605'

r = requests.session()
html_doc = r.get(url).text
soup = bs4.BeautifulSoup(html_doc,'html.parser')
plain_text = soup.h3.string
cipher_text = hashlib.md5(plain_text.encode()).hexdigest()
resp = r.post(url= url, data={'hash': cipher_text})
print(resp.text)


```

Run it and the flag will appear:

![image-20210208202512877](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20210208202512877.png)

Guess the difficulty is easy