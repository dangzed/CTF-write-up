Truy cập vào bất kì thư mục nào URL sẽ có dạng

> ?files=[tên thư mục]

Và khi ta muốn truy cập bằng quyền admin(connected as admin) url sẽ trở thành

> /admin/...

và path này cần authentication. Thử exploit với lỗ hổng LFI bằng cách nhập URL

> ?files=../admin

sẽ hiện ra file `index.php`, trong đó có password cần tìm