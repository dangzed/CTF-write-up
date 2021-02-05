Do gợi ý là doc về git, ta truy cập vào đường dẫn `/.git`, sẽ thấy 1 cấu trúc thư mục dạng git được hiển thị

Thử dùng git clone không hiệu quả do nó không phải git repository, ta dùng tool [gitdumper](https://github.com/internetwache/GitTools) để kéo chúng về máy. Gõ command line `git log`, ta thấy ở lần commit gần nhất 2 file `index.php` và `config.php` đã bị xóa đi. Dùng `git stash` để khôi phục lại 2 file đó, nội dung file `index.php` như sau:

```php+HTML
<!doctype html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Coffee Database</title>
	<link rel="stylesheet" href="css/style.css" />
</head>

<body>
	<form action='' method="POST">
		<img src='./image/logo.png' id='logo'>
		<h2>Coffee Database</h2>
		<?php
			include('./config.php');
			if(isset($_POST['username']) && isset($_POST['password'])){
				if ($_POST['username'] == $username && md5($_POST['password']) == md5($password)){
					echo "<p id='left'>Welcome  ".htmlentities($_POST['username'])."</p>";
					echo '<input type="submit" value="LOG IN" href="./index.php" class="button" />';
				}
				else{
					echo "Unknown user or password";
					echo "<input type='submit' class='button' value='Back' />";
				}
			}
			else{
				?>
				<input type="text" name="username" class="text-field" placeholder="Username" />
	    		<input type="password" name="password" class="text-field" placeholder="Password" />
	   			<input type="submit" value="LOG IN" class="button" />
				<?php
			}
		?>

	</form>
</body>
</html>
```

Vậy server sẽ lấy input ta nhập vào, SHA hash password và so sánh với 2 biến username, password trong `config.php`. Do không thể decrypt SHA, ta sẽ nhảy đến thời điểm password chưa được mã hóa, chính là branch có commit nội dung : `changed password` bằng lệnh git checkout [tên branch] -f.  Vào lại file `config.php`, ta đã có username và password cần tìm