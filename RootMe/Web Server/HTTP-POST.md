Inspect element trang web, ta thấy đoạn code sau:

```javascript
<form action="" method="post" onsubmit="document.getElementsByName('score')[0].value = Math.floor(Math.random() * 1000001)">
            <input type="hidden" name="score" value="-1">
            <input type="submit" name="generate" value="Give a try!">
</form>
<input type="hidden" name="score" value="-1">
<input type="submit" name="generate" value="Give a try!">
```

sửa lại code thành Math.random()*10000000,  ấn `Give a try!` lần nữa và ta sẽ có password