# python_blockchain_app

Hướng dẫn đơn giản để phát triển ứng dụng blockchain từ đầu bằng Python.

## Blockchain là gì? Nó được triển khai như thế nào? Và nó hoạt động như thế nào?

Vui lòng đọc [hướng dẫn triển khai từng bước](https://www.ibm.com/developerworks/cloud/library/cl-develop-blockchain-app-in-python/index.html) để biết câu trả lời của bạn :)

## Hướng dẫn chạy

Clone the project,

```sh
$ git clone https://github.com/satwikkansal/python_blockchain_app.git
```

Install the dependencies,

```sh
$ cd python_blockchain_app
$ pip install -r requirements.txt
```

Start a blockchain node server,

```sh
# Windows users can follow this: https://flask.palletsprojects.com/en/1.1.x/cli/#application-discovery
$ export FLASK_APP=node_server.py
$ flask run --port 8000
```

Một phiên bản nút blockchain của tôi hiện đang hoạt động tại cổng 8000.


Chạy ứng dụng trên một phiên bản thiết bị đầu cuối khác,

```sh
$ python run_app.py
```

Ứng dụng phải được thiết lập và chạy tại [http://localhost:5000](http://localhost:5000).

Dưới đây là một vài ảnh chụp màn hình

1. Đăng một số nội dung

![image.png](https://github.com/satwikkansal/python_blockchain_app/raw/master/screenshots/1.png)

2. Yêu cầu nút khai thác

![image.png](https://github.com/satwikkansal/python_blockchain_app/raw/master/screenshots/2.png)

3. Đồng bộ hóa với chuỗi để cập nhật dữ liệu

![image.png](https://github.com/satwikkansal/python_blockchain_app/raw/master/screenshots/3.png)

Để thử nghiệm bằng cách tạo ra nhiều nút tùy chỉnh, hãy sử dụng điểm cuối `register_with/` để đăng ký một nút mới.

Đây là một kịch bản mẫu mà bạn có thể muốn thử,

```sh
# already running
$ flask run --port 8000 &
# spinning up new nodes
$ flask run --port 8001 &
$ flask run --port 8002 &
```

Bạn có thể sử dụng các yêu cầu cURL sau để đăng ký các nút ở cổng `8001` và `8002` với `8000` đang chạy.

```sh
curl -X POST \
  http://127.0.0.1:8001/register_with \
  -H 'Content-Type: application/json' \
  -d '{"node_address": "http://127.0.0.1:8000"}'
```

```sh
curl -X POST \
  http://127.0.0.1:8002/register_with \
  -H 'Content-Type: application/json' \
  -d '{"node_address": "http://127.0.0.1:8000"}'
```

Điều này sẽ giúp nút ở cổng 8000 nhận biết được các nút ở cổng 8001 và 8002, đồng thời giúp các nút mới hơn đồng bộ chuỗi với nút 8000 để chúng có thể chủ động tham gia vào quá trình khai thác sau khi đăng ký.

Để cập nhật nút mà ứng dụng giao diện người dùng đồng bộ hóa (mặc định là cổng localhost 8000), hãy thay đổi trường `CONNECTED_NODE_ADDRESS` trong tệp [views.py](/app/views.py).

Sau khi thực hiện tất cả những điều này, bạn có thể chạy ứng dụng, tạo giao dịch (đăng tin nhắn qua giao diện web) và sau khi khai thác giao dịch, tất cả các nút trong mạng sẽ cập nhật chuỗi. Chuỗi các nút cũng có thể được kiểm tra bằng cách khởi tạo điểm cuối `/chain` bằng cURL.

```sh
$ curl -X GET http://localhost:8001/chain
$ curl -X GET http://localhost:8002/chain
```
