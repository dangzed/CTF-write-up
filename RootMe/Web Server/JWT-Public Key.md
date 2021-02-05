Bài này làm theo công thức đã cho trong doc, không có gì mới, chẳng qua trong quá trình copy paste có thể bị rơi mất dữ liệu nào đó nên mãi mới làm được.

Tham khảo [Link](https://www.nccgroup.com/uk/about-us/newsroom-and-events/blogs/2019/january/jwt-attack-walk-through/) để giải chall này

<b>Kiến  thức cần biết</b>: Tấn công jwt với thuật toán mã hóa RSA256, ta sửa `header` thành thuật HSA256, gán secret key là public key của RSA sẽ bypass được phân đoạn verify signature  

Các bước như sau:

- GET endpoint /key để lấy public key, để đúng format của nó những chỗ xuống dòng, chỉ xóa đi dấu ngoặc kép và dấu phẩy chặn ở giữa
- POST endpoint /auth với cặp key-value  "username=dang"  để lấy jwt, thực hiện y hệt như [Link](https://www.nccgroup.com/uk/about-us/newsroom-and-events/blogs/2019/january/jwt-attack-walk-through/) để ra được jwt của admin
- POST endpont /admin với jwt của admin để lấy flag