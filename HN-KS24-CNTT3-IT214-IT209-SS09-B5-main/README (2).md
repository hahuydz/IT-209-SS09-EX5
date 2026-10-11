# Bài 5: Triển khai Nginx bằng Docker Compose

## 1. Mục tiêu

-   Làm quen với Docker Compose.
-   Khai báo và khởi chạy một dịch vụ Nginx bằng file
    `docker-compose.yml`.
-   Kiểm tra trạng thái container và xác nhận web server phản hồi HTTP
    thành công.

## 2. Cấu trúc thư mục

``` text
bai5/
├── docker-compose.yml
└── README.md
```

Các ảnh chụp màn hình minh chứng có thể được lưu thêm trong thư mục này.

## 3. Cấu hình Docker Compose

Nội dung file `docker-compose.yml`:

``` yaml
services:
  web:
    image: nginx:alpine
    ports:
      - "8088:80"
```

### Giải thích

-   `services`: khai báo các dịch vụ được Compose quản lý.
-   `web`: tên dịch vụ Nginx.
-   `image: nginx:alpine`: sử dụng image Nginx phiên bản Alpine gọn nhẹ.
-   `ports: "8088:80"`: ánh xạ cổng `8088` trên máy host sang cổng `80`
    trong container.

**Lưu ý:** Bài thực hành sử dụng cổng host `8088` vì cổng `8082` đang
được dịch vụ MovieBooking sử dụng trên máy. Việc đổi cổng host giúp
tránh xung đột mà không cần dừng các container khác.

## 4. Triển khai

Mở Terminal tại thư mục `bai5`.

### Bước 1: Kiểm tra cấu hình

``` bash
docker compose config
```

Lệnh này kiểm tra và hiển thị cấu hình Compose sau khi được chuẩn hóa.

### Bước 2: Khởi chạy dịch vụ

``` bash
docker compose up -d
```

Tùy chọn `-d` giúp container chạy ở chế độ nền.

### Bước 3: Kiểm tra trạng thái

``` bash
docker compose ps
```

Container `bai5-web-1` cần có trạng thái đang chạy và hiển thị ánh xạ
cổng tương tự:

``` text
0.0.0.0:8088->80/tcp
```

### Bước 4: Kiểm tra phản hồi HTTP

``` bash
curl -I http://localhost:8088
```

Kết quả kiểm thử thành công:

``` text
HTTP/1.1 200 OK
Server: nginx/1.31.6
Content-Type: text/html
```

Có thể mở trình duyệt tại địa chỉ:

<http://localhost:8088>

Trang mặc định **Welcome to nginx!** sẽ xuất hiện nếu dịch vụ hoạt động
bình thường.

## 5. Kết quả

-   File `docker-compose.yml` hợp lệ.
-   Docker Compose khởi chạy thành công dịch vụ `web`.
-   Container Nginx hoạt động.
-   Cổng host `8088` được ánh xạ tới cổng container `80`.
-   Lệnh kiểm tra HTTP trả về `200 OK`.

## 6. Dừng dịch vụ

Khi hoàn tất bài thực hành, chạy:

``` bash
docker compose down
```

Lệnh này dừng và xóa container cùng network do Compose của bài thực hành
tạo ra. Nó không xóa image `nginx:alpine` và không tác động đến các
container MovieBooking khác.

## 7. Minh chứng

Nên đính kèm các ảnh sau khi nộp bài:

1.  Nội dung file `docker-compose.yml`.
2.  Kết quả lệnh `docker compose ps`.
3.  Trang Nginx mở tại `http://localhost:8088`.
4.  Kết quả lệnh `curl -I http://localhost:8088`.

------------------------------------------------------------------------

**Kết luận:** Bài thực hành đã triển khai thành công Nginx bằng Docker
Compose và xác nhận dịch vụ hoạt động thông qua phản hồi HTTP `200 OK`.
