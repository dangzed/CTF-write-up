Đọc doc của chall này có những điểm sau cần lưu ý :

- Chall này cần sự kết hợp của 2 kĩ thuật: `path normalization` và `path truncation`

- `Path truncation`: PHP ver 5.3 trở xuống giới hạn string tối đa 4096 kí tự, nếu có 1 string nào ngoài giới hạn đó nó sẽ tự động cắt bỏ những kí tự thừa đi
- `Path normalization`: /etc/passwd/ và /etc/password/. đều có thể coi như mở 1 file, không còn bị lỗi `ENDOFDIR : not an directory`
- Khi tấn công bằng phương pháp này, đường dẫn phải có số kí tự `/` và số kí tự `.` khác nhau, kết thúc phải luôn là dấu `.` 

Vậy tóm lại ta thử với đường dẫn sau:

> ?page=admin.html/[2027 lần kí tự "/."]

có thể  viết 1 script đơn giản để sinh ra 2027 chuỗi kí tự "/." như:

```javascript
tmpString = '';
for(i=0;i<2027;i++) tmpString += '/.';
console.log(tmpString);
```

Lưu ý: ở đây lấy số 2027 là số sẵn ở trong doc của rootme, còn có thể số tùy ý cũng được miễn tổng kí tự của đường dẫn lớn hơn 4096