Bài này tiêu đề là CRLF, cùng với gợi ý `Inject false data in the journalisation log`, ta nghĩ đến việc làm sao để log ra 1 lần admin đăng nhập giả. Do nhập gì vào ô username nó cũng báo `[username] failed to authenticate`, nên ta sẽ thêm tham số như sau vào đường dẫn:

`?username=admin%20authenticated.%0D%0Aguest&password=100`

ở đây: %20 là dấu cách, %0D là carriage return \r, %0A là line feed \n, dùng để tạo ra 1 dòng mới(chính là dòng guest failed...),

còn password chúng ta lấy tùy ý không quan trọng

Thẻ h3 chứa password đã hiện ra