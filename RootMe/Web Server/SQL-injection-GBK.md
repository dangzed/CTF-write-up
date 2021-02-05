Bài này làm mình mất nhiều thời gian nhất, không phải vì không biết cách hay không hiểu cơ chế. Hint đã có sẵn: `GBK` và `do you speak chinese?`, search gg với từ khóa `GBK` ta biết đó là 1 cách để bypass hàm `addslash()`, đại khái là khi nhập input 

> ' or 1=1-- -

thì nó sẽ trở thành:

> \\' or 1=1-- -

kí tự `'` encode sẽ trở thành `%27`, `\` là `%5c`, và trong hệ mã hóa GBK tổ hợp `%bf%5c` là 1 chữ cái Trung Quốc, vậy có thể nghĩ cách bypass bằng payloads sau trong Burp Suite:

> login=%bf%27+or+1%3D1--+-&password=123

Trước đây mãi mình không làm được là do có thử kiểu gì đi nữa nó vẫn trả về `   Erreur d'identification`, nhưng hóa ra với payloads đúng, có 1 nút mới hiện ra:

![image-20201028173400633](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20201028173400633.png)

Bấm nút `Follow redirection`, ta sẽ tới được trang `/logged.php` và nhận được flag