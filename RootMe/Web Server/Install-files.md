Trong inspect element có đường dẫn `/phpbb`, thử truy cập vào nó sẽ chưa hiển thị gì đặc biệt

Search gg với từ khóa phpbb install files, ta biết thêm 1 lỗi: người build web có thể quên không xóa file `install.php` trong thư mục `install`, gây nguy hiểm do file này lưu trữ rất nhiều thông tin bảo mật trong khi ai cũng truy cập vào được.

Nhập đường dẫn `/phpbb/install/install.php` và sẽ lấy được password