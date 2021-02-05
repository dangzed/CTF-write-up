Đọc source code của chương trình:

```php+HTML
<html>
<body>
 <form action="index.php" class="authform" method="post" accept-charset="utf-8">
        <fieldset>
            <legend>Unbreakable Random</legend>
            <input type="text" id="s" name="s" value="" placeholder="seed" />
            <input type="text" id="h" name="h" value="" placeholder="hash" />
            <input type="submit" name="submit" value="Check" />

        <div class="return-value" style="padding: 10px 0">&nbsp;</div>
        </fieldset>
        </form>
<?php
function gen_secured_random() { // cause random is the way
    $a = rand(1337,2600)*42;
    $b = rand(1879,1955)*42;

    $a < $b ? $a ^= $b ^= $a ^= $b : $a = $b;

    return $a+$b;
}

function secured_hash_function($plain) { // cause md5 is the best hash ever
    $secured_plain = sanitize_user_input($plain);
    return md5($secured_plain);
}

function sanitize_user_input($input) { // cause someone told me to never trust user input
    $re = '/[^a-zA-Z0-9]/';
    $secured_input = preg_replace($re, "", $input);
    return $secured_input;
}

if (isset($_GET['source'])) {
    show_source(__FILE__);
    die();
}


require_once "secret.php";

if (isset($_POST['s']) && isset($_POST['h'])) {
    $s = sanitize_user_input($_POST['s']);
    $h = secured_hash_function($_POST['h']);
    $r = gen_secured_random();
    if($s != false && $h != false) {
        if($s.$r == $h) {
            print "Well done! Here is your flag: ".$flag;
        }
        else {
            print "Fail...";
        }
    }
    else {
        print "<p>Hum ...</p>";
    }
}
?>
<p><em><a href="index.php?source">source code</a></em></p>
</body>
</html>
```

Vậy ở đây nó lấy chuỗi `seed` nối với chuỗi số random sinh ngẫu nhiên, so sánh với chuỗi md5(`hash`), nếu bằng nhau thì có flag.

Theo như doc của rootme, có 1 trường hợp loose comparison của php như sau:

> "0e12345" == "0e54321"

Vậy cần tìm chuỗi `hash` sao cho sau khi dùng md5, ta sẽ có được 1 chuỗi bắt đầu bằng 0e và còn lại chỉ toàn là số. Vậy là vẫn phải brute force, cứ tưởng không cần . 

Do quá lười viết code, mình search thử `md5 string start with 0e`, vậy mà ra kết quả thật : [link](https://www.whitehatsec.com/blog/magic-hashes/)

Vậy điền vào form như sau:

> seed: 0e462097431906509019562988736854
>
> hash: 240610708

Và sẽ lấy được flag