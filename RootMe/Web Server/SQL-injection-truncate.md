1 lỗ hổng rất hay của sql mà qua bài này mình mới biết : SQL truncation. Theo doc, cách khai thác nó như sau:

- Khi so sánh 2 string trong database, dấu cách thường bị bỏ qua, nghĩa là 'admin' và 'admin       ' là bằng nhau, do đó db sẽ từ chối insert query
- Khi string vượt quá số lượng kí tự được định nghĩa sẵn, nó sẽ cắt đi phần kí tự thừa

Rồi, bắt đầu tấn công trên rootme!

Trên trang `register`, inspect element ta thấy câu truy vấn tạo bảng user:

```mysql
CREATE TABLE IF NOT EXISTS user(   
	id INT NOT NULL AUTO_INCREMENT,
    login VARCHAR(12),
    password CHAR(32),
    PRIMARY KEY (id)
);
```

vậy giới hạn của login là 12 chữ cái. Điền vào form như sau:

```
Pseudo: 'admin       harm'(có 7 dấu cách tất cả)
Password:'12345678'
```

thông báo hiện lên User saved!, vậy thành công rồi. Chuyển qua trang Administration, nhập password vừa tạo sẽ có được flag

