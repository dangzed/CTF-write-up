Khi ta thay attr `href` sang 1 URL khác mà không cung cấp tham số `h`, chúng ta thấy thông báo đỏ `incorrect hash`, vậy tham số h ở đây có lẽ là chuỗi hash của 1 cái gì đó, và nhìn dạng thì giống md5 hash. Ta thử dùng [md5 hash tools](https://www.md5hashgenerator.com/) thì phát hiện ra tham số h chính là chuỗi md5 hash của url trước đó. Sửa 1 URL bất kì thành ``` href="?url=https://example.com&h=c984d06aafbecf6bc55569f964148ea3"```, reload trang web và dùng BurpSuite bắt request, ta lấy được password trong response 
