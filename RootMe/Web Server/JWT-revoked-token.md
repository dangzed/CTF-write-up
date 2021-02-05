Đọc source code của rootme:

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
from flask import Flask, request, jsonify
from flask_jwt_extended import JWTManager, jwt_required, create_access_token, decode_token
import datetime
from apscheduler.schedulers.background import BackgroundScheduler
import threading
import jwt
from config import *
 
# Setup flask
app = Flask(__name__)
 
app.config['JWT_SECRET_KEY'] = SECRET
jwtmanager = JWTManager(app)
blacklist = set()
lock = threading.Lock()
 
# Free memory from expired tokens, as they are no longer useful
def delete_expired_tokens():
    with lock:
        to_remove = set()
        global blacklist
        for access_token in blacklist:
            try:
                jwt.decode(access_token, app.config['JWT_SECRET_KEY'],algorithm='HS256')
            except:
                to_remove.add(access_token)
       
        blacklist = blacklist.difference(to_remove)
 
@app.route("/web-serveur/ch63/")
def index():
    return "POST : /web-serveur/ch63/login <br>\nGET : /web-serveur/ch63/admin"
 
# Standard login endpoint
@app.route('/web-serveur/ch63/login', methods=['POST'])
def login():
    try:
        username = request.json.get('username', None)
        password = request.json.get('password', None)
    except:
        return jsonify({"msg":"""Bad request. Submit your login / pass as {"username":"admin","password":"admin"}"""}), 400
 
    if username != 'admin' or password != 'admin':
        return jsonify({"msg": "Bad username or password"}), 401
 
    access_token = create_access_token(identity=username,expires_delta=datetime.timedelta(minutes=3))
    ret = {
        'access_token': access_token,
    }
   
    with lock:
        blacklist.add(access_token)
 
    return jsonify(ret), 200
 
# Standard admin endpoint
@app.route('/web-serveur/ch63/admin', methods=['GET'])
@jwt_required
def protected():
    access_token = request.headers.get("Authorization").split()[1]
    with lock:
        if access_token in blacklist:
            return jsonify({"msg":"Token is revoked"})
        else:
            return jsonify({'Congratzzzz!!!_flag:': FLAG})
 
 
if __name__ == '__main__':
    scheduler = BackgroundScheduler()
    job = scheduler.add_job(delete_expired_tokens, 'interval', seconds=10)
    scheduler.start()
    app.run(debug=False, host='0.0.0.0', port=5000)
```

Logic code ở đây là: sau khi POST request với tài khoản admin, 1 token được sinh ra và cho vào trong blacklist. Truy cập vào endpoint `/admin`, server kiểm tra xem token có ở trong blacklist không, nếu không thì mới ra flag.



Ở đây có 1 dòng code khá lỏng lẻo:

> access_token = request.headers.get("Authorization").split()[1]

Nó cho thấy bất kì request nào, access token cũng được lấy bằng cách split string value trong header Authorization, get phần tử có index 1. Vào đọc doc của jwt_required, ta thấy nó xác định jwt theo cách khác chặt chẽ hơn nhiều, từ đó mở ra cơ hội bypass cái blacklist này. Việc của chúng ta là cung cấp 1 jwt hợp lệ với format như sau:

> abc abcd, Bearer YOURTOKEN

thì `jwt_required` cho phép format dạng này, đồng thời phép thử trong code sẽ return ra kết quả `abcd` đương nhiên không có trong blacklist rồi, từ đó sẽ lấy được password