# bai5_Phat_trien_ung_dung_tren_ma_nguon_mo
BÀI TẬP LỚN
môn: phát triển ứng dụng với mã nguồn mở - tee0421
bt1: Ubuntu + Docker: DÙNG DOCKER ĐỂ BUILD myapi
bt2: django (python): WEB QUẢN LÝ TIỆM CẦM ĐỒ
bt3: wordpress (php) + mariadb + phpmyadmin (chỉ để xem các tables)
bt4: wordpress + n8n + bot telegram+gemini: auto đăng bài bằng cách chát
bt5:
  - lý thuyết: 
    + docker là gì? 
    + các keyword được sử dụng trong docker-compose.yml
      để mô tả 1 service, network, volume,...
      liệt kê + ý nghĩa của từ khoá đó + ví dụ minh hoạ
    + ưu điểm khi triển app sử dụng docker là gì?
    + dùng docker: tạo app, test app OK trên laptop cá nhân
      giờ muốn triển khai app này trên máy chủ thật ko có internet
      thì các bước cần làm là?
  - thực hành áp dụng: APP MONITOR + ALERT DATA REALTIME
    sử dụng docker compose có nhiều serivce 
    và các thành phần cần thiết để tạo thành ứng dụng:
     + nodered liên tục lấy dữ liệu từ nguồn nào đó (chứng khoán, thời tiết, giá vàng,...)
       nguồn thực tế, số liệu luôn động sau thời gian ngắn
     + nodered lưu trữ dữ liệu vào 2 database: mariadb để lưu giá trị tức thời
       lưu lịch sử vào influxdb
     + sử dụng grafana để trực quan hoá dữ liệu: vẽ biểu đồ
     + sử dụng nginx để làm webserver
       chạy 1 trang web html+js+css làm front-end
       js: lấy dữ liệu tức thời trong mariadb qua (ajax | socket) 
           gọi api (api tự build bằng Flask giống bt1)
           api trả về giá trị tức thời trong mariadb
           hiển thị lên web, auto hiển thị số mới khi thay đổi
       sử dụng iframe để gọi grafana
       hiển thị biểu đồ dữ liệu lịch sử của thông số đã lưu
     + QUAN SÁT DỮ LIỆU LỊCH SỬ => GIÁ TRỊ BẤT THƯỜNG
       (VD MIỀN A..B: OK, DƯỚI A: ALERT LOW, TRÊN B: ALERT HIGH)
     + nodered: kết hợp bot Telegram
       khi dữ liệu not OK, thì gửi tin nhắn từ bot => group trên telegram
       group đã add bot vào: (nhóm đã có 2 người), add thêm 1875746636 thành 3 người
       mỗi khi bot gửi dữ liệu vào nhóm: mọi member of group đều nhận đc
       nội dung alert: tường minh, có value gây alert

     xuất tất cả các container ra file nén.
     xoá mọi container đang chạy
     load lại các container  từ file nén để khôi phục các container đã xoá
=========
quá trình làm: chụp ảnh lại, mô tả cho ảnh
  lưu vào trong github => paste link access public của repo: vào file excel online
=====================

cả 5 bài tập:
biên tập lại xíu để phù hợp với bản print
đóng quyển, ko cần bìa xanh, ko cần giấy bóng kính
header+footer của các trang giấy: có tên + masv, bài tập lớp Môn, số trang
trang 1 có đầy đủ thông tin cá nhân.

lưu ở bm để chấm điểm.


### bài làm
- Tạo thư mục BT5
Chạy:
mkdir ~/bt5-monitor
cd ~/bt5-monitor
<img width="452" height="118" alt="image" src="https://github.com/user-attachments/assets/518f8d41-f2d8-4a56-8c96-d9d141df50d9" />

- Tạo file docker-compose.yml : nano docker-compose.yml
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0c32bf41-1b53-4cae-8ad4-2dd9b492867f" />

- Kiểm tra file: cat docker-compose.yml
<img width="1713" height="873" alt="image" src="https://github.com/user-attachments/assets/10ebc569-55d6-4344-bfa1-768b9085d1d1" />

- Khởi động các container: docker compose up -d
<img width="1651" height="777" alt="image" src="https://github.com/user-attachments/assets/53bb8ab4-1351-4b81-862c-9e02d926231b" />

- Kiểm tra container: docker ps
<img width="1653" height="247" alt="image" src="https://github.com/user-attachments/assets/c7943311-39e6-4781-8e35-ec8ba53062dd" />

- Kiểm tra Node-RED

Mở trình duyệt: http://IP-Ubuntu:1880
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3727e72d-eee1-4580-a872-a024fe2b6c07" />

- Kiểm tra Grafana

Mở: http://localhost:3000
<img width="954" height="1015" alt="image" src="https://github.com/user-attachments/assets/f847f00b-514f-4bba-bca6-8820aed43289" />

- Kiểm tra InfluxDB

Mở: http://localhost:8086
<img width="959" height="1016" alt="image" src="https://github.com/user-attachments/assets/8aba50a7-1399-486a-b570-5ee7175736b9" />

- Kiểm tra MariaDB

Chạy: docker exec -it mariadb_bt5 mariadb -u root -p
<img width="838" height="248" alt="image" src="https://github.com/user-attachments/assets/b1da6f54-cf6c-4a2b-9cf7-097882a31bdb" />

- Kiểm tra database:

SHOW DATABASES;
<img width="433" height="307" alt="image" src="https://github.com/user-attachments/assets/78543aa0-096e-434f-80b8-c64ad898dd23" />

- Chọn database
USE monitordb;
<img width="338" height="69" alt="image" src="https://github.com/user-attachments/assets/3f3c6fbf-2f4c-45b4-97f8-74bac28ff4ca" />

- Tạo bảng realtime_data
<img width="507" height="235" alt="image" src="https://github.com/user-attachments/assets/1f8b3b8d-1ea9-488c-8284-7d1ee66260c0" />

- Kiểm tra bảng đã tạo
SHOW TABLES;
<img width="340" height="219" alt="image" src="https://github.com/user-attachments/assets/99922131-3580-4e35-a6cc-2757c75a4f3d" />

- Chèn thử dữ liệu mẫu
<img width="674" height="120" alt="image" src="https://github.com/user-attachments/assets/d282076b-ca28-4eef-b785-5ae774c24cbc" />

- Kiểm tra dữ liệu
<img width="506" height="207" alt="image" src="https://github.com/user-attachments/assets/f4f83a9d-baa8-4709-8cfd-d7f45ddea278" />

- Tạo thư mục Flask API

mkdir flask-api
cd flask-api

<img width="531" height="110" alt="image" src="https://github.com/user-attachments/assets/f1a9d95c-1479-4689-8f56-31e753826e9c" />

Tạo file app.py
nano app.py
<img width="1716" height="881" alt="image" src="https://github.com/user-attachments/assets/43938e77-2411-4563-8a9a-ce1ece493c98" />

- Tạo requirements.txt
nano requirements.txt
<img width="1716" height="877" alt="image" src="https://github.com/user-attachments/assets/b56e23af-1ce9-4565-9e51-d5efc0c4e5e3" />

- Tạo Dockerfile
nano Dockerfile
<img width="753" height="877" alt="image" src="https://github.com/user-attachments/assets/516e00cc-bed7-4d55-9d13-6964cc6c718a" />
- Quay về thư mục monitor
cd ..

- Mở docker-compose.yml
nano docker-compose.yml
Thêm service Flask vàn nginx vào dưới phần Node-RED
<img width="755" height="876" alt="image" src="https://github.com/user-attachments/assets/af1625eb-77d4-48bd-9408-3e0db2999def" />

- Build lại
docker compose up -d --build
<img width="1717" height="877" alt="image" src="https://github.com/user-attachments/assets/920d6af8-db91-4344-8888-2fdbef5d5ed4" />

- Kiểm tra Flask
docker ps
<img width="1650" height="334" alt="image" src="https://github.com/user-attachments/assets/ed576243-b428-47d2-a5f5-af83479f036e" />

- Kiểm tra API
Chạy: curl localhost:5000/api/data
<img width="666" height="67" alt="image" src="https://github.com/user-attachments/assets/5df5dbf0-4736-4c15-979b-da8601d34f2e" />

- Node-RED tự động lấy dữ liệu thời tiết
- Mở Node-RED
Tìm và cài lần lượt
MySQL : node-red-node-mysql
InfluxDB: node-red-contrib-influxdb
Telegram: node-red-contrib-telegrambot
<img width="1919" height="1012" alt="image" src="https://github.com/user-attachments/assets/6ff26149-5732-4b68-827d-d276a7978be5" />

- Kéo node:

Inject
<img width="692" height="795" alt="image" src="https://github.com/user-attachments/assets/fc8aa849-6b02-4634-b614-8d114c350990" />

- KÉO NODE HTTP REQUEST : nối Inject với HTTP Request
<img width="884" height="808" alt="image" src="https://github.com/user-attachments/assets/58803e85-4cba-4a25-a025-dffbf51e893c" />



- KÉO FUNCTION GetTemp: HTTP Request với GetTemp
- <img width="825" height="520" alt="image" src="https://github.com/user-attachments/assets/76ff6a38-1d8e-48ea-b6e9-0b2fb994092c" />

- KÉO DEBUG
Kéo:
Debug
Nối:
GetTemp
 ↓
Debug: KÉO FUNCTION SQL

Kéo:

Function

Nối từ:

GetTemp
 ↓
Function SQL
<img width="953" height="1017" alt="image" src="https://github.com/user-attachments/assets/520aa3b9-0a09-4064-b093-159d3fbde4a5" />

- KÉO MYSQL

Kéo:

mysql

Nối:

SQL Insert
 ↓
MySQL

<img width="644" height="801" alt="image" src="https://github.com/user-attachments/assets/a08b63dd-ad36-4966-b1ea-dc3fd69820a2" />

- KÉO FUNCTION INFLUX

Kéo:

Function

Nối từ:

GetTemp
 ↓
Function Influx
<img width="955" height="1021" alt="image" src="https://github.com/user-attachments/assets/c44e48c0-f5d2-4690-b9e4-7a979f960ecc" />

- truy cập influxdb để lấy token
<img width="958" height="1020" alt="image" src="https://github.com/user-attachments/assets/cb85d309-463b-46ff-bb74-4df8272c044d" />

Cấu hình ở grafana
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/05683bbc-14b1-48b7-89e1-1a4edbacaaad" />

KÉO SWITCH

Kéo:

Switch

Nối:

GetTemp
 ↓
Switch
<img width="721" height="975" alt="image" src="https://github.com/user-attachments/assets/aed34461-f744-4d01-bf69-6d1f84861e92" />

- KÉO FUNCTION ALERT

Kéo:

Function

Nối cả 2 output của Switch vào Function này.
<img width="963" height="1017" alt="image" src="https://github.com/user-attachments/assets/214cd5d7-867b-4a14-86a2-64172db4394e" />

KÉO TELEGRAM SENDER

Kéo:

telegram sender

Nối:

Function Alert
 ↓
Telegram Sender
<img width="948" height="1015" alt="image" src="https://github.com/user-attachments/assets/6132bf8b-25ed-47b7-9af0-45145499fdaf" />

deploy
<img width="1919" height="1020" alt="image" src="https://github.com/user-attachments/assets/b917e70b-83a2-4c53-be3b-05713f2c7263" />

- Cấu hình Influxdb
<img width="1919" height="1020" alt="image" src="https://github.com/user-attachments/assets/e17df305-ef39-43a0-9162-27101538c311" />

- cấu hình grafana
- dashbroards -> new -> new dashbroards
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/bba68e31-cceb-4e31-ab2d-adb647c3b821" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4581124a-3ad7-4719-b6ec-d478e4d83b60" />

- Tạo thư mục frontend
Tạo thư mục: mkdir -p nginx/html
<img width="690" height="94" alt="image" src="https://github.com/user-attachments/assets/b14b0ebe-a902-4924-a4a0-424ee95524ae" />

- Tạo file index.html: nano ~/bt5-monitor/nginx/html/index.html
<img width="1716" height="963" alt="image" src="https://github.com/user-attachments/assets/d02cbb59-b4f0-43c9-9978-0972f9de1d7f" />

<img width="1653" height="180" alt="image" src="https://github.com/user-attachments/assets/40fd8fd2-75fb-4a0a-8dc5-533a03c07de3" />

- kết quả
- Truy Cập: http://192.168.18.134:8088
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5065bb49-5e1f-4db1-b5f2-ee49f5d374da" />



