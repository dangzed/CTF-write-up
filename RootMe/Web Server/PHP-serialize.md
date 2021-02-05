Bài này đầu tiên mình không để ý code lắm, chỉ xem thấy sau khi tick vào ô `autologin`, gửi request với tài khoản guest thì response trả về là đính kèm 1 cookie. URLdecode cookie đó ra kết quả:

> a:2:{s:5:"login";s:5:"guest";s:8:"password";s:64:"84983c60f7daadc1cb8698621f802c0d9f9a3c3c295c810748fb048115c186ec";}

Vì đoạn code có nói đến SHA nên mình nghĩ chuỗi 64 kí tự kia là SHA hash của chuỗi gì đấy, thử với chuỗi `guest` thì khớp,vậy ta đổi chuỗi serialized trên thành như sau:

> a:2:{s:5:"login";s:10:"superadmin";s:8:"password";s:64:"8c6976e5b5410415bde908bd4dee15dfb167a9c873fc4bb8a81f6f2ab448a918";}

(trong đó s64 là chuỗi hash của `superadmin`)

urlencode nó và gán vào giá trị cookie đính kèm, reload lại trang xem có ra gì không. Kết quả fail toàn tập

Đến đây xem gợi ý trên mạng, thì hóa ra có 2 điều đáng chú ý về lỗ hổng tên là `loose comparison` như sau: 

- PHP giống Python và JS, không có kiểu dữ liệu, nên dấu so sánh `==` sẽ không chính xác trong nhiều trường hợp. Trường hợp dễ exploit nhất chính là `string == 0` hoặc `string == true` luôn cho ra kết quả đúng
- Tuy nhiên nếu chỉ dùng phương thức POST của php thì nó sẽ tự động nộp dữ liệu dưới dạng string, điều đó khiến `loose comparision` vô nghĩa. Nhưng hàm serialize() lại có thể giữ nguyên kiểu dữ liệu truyền vào (trùng hợp đó chính là hàm dùng trong code)

Đồng thời đọc lại code phát hiện ra, nếu mình đính kèm cookie autologin, trang web sẽ không lấy data từ phương thức POST nữa và cũng không có SHA cái gì cả, nó lấy trực tiếp data từ cookie. Vậy ta sửa lại chuỗi serialized như sau:

> a:2:{s:5:"login";s:10:"superadmin";s:8:"password";i:0";}

i:0 nghĩa là số 0 integer

hoặc

> a:2:{s:5:"login";s:10:"superadmin";s:8:"password";b:1";}

b:1 nghĩa là boolean true

urlencode và đính kèm cookie, tuy nhiên có vẻ cách 1 không hiệu quả, tạm thời mình chưa hiểu vì sao