---
layout: post
title:  "Giới thiệu và cài đặt MongoDB"
date:   2017-04-29 14:02:57 +0700
categories: jekyll update
---
![Giới thiệu và cài đặt MongoDB](https://ongxuanhong.files.wordpress.com/2016/03/mongodb-hortonwork.png?w=665)
> Giới thiệu và cài đặt MongoDB

Khi nói đến NoSQL database, ta sẽ nghe đến một cái tên quen thuộc đó là MongoDB. Đây là một open-source document database, được viết bằng ngôn ngữ c++, được dùng để lưu trữ và truy xuất dữ liệu lớn Big Data. Tôi mới làm quen với MongoDB nên làm một cái note để xem lại khi cần cài đặt hay nắm bắt nhanh các khái niệm cơ bản của cơ sở dữ liệu (CSDL) này. Bài viết gồm hai phần: giới thiệu và cài đặt.


Giới thiệu

MongoDB là một cross-platform (Linux, Mac OS, Windows), là document oriented database (CSDL hướng tài liệu) cung cấp một hệ thống có hiệu suất và tính sẵn sàng cao (high performance, high availability) cũng như khả năng dễ dàng mở rộng. MongoDB hoạt động dựa trên các khái niệm cơ bản về bộ sưu tập (collection) và tài liệu (document). Dưới đây là các khái niệm cơ bản.

Database: là một container vật lý chứa các collection. Mỗi database sẽ có hệ thống file và tập tin tương ứng khi cài đặt trên các hệ điều hành cụ thể. Một MongoDB server đơn lẻ thường có nhiều database.
Collection: là một group các MongoDB document. Khái niệm này tương đương với table trong RDBMS. Một collection tồn tại bên trong một database đơn lẻ. Các collection không tạo ra một lược đồ (schema). Các document bên trong một collection có thể có nhiều trường (fields/ columns). Thông thường, tất cả các document trong một collection thì tương tự và liên quan với nhau.
Document: là tập các cặp key-value. Các document có lược đồ rất linh hoạt (dynamic schema). Nghĩa là các document trong cùng một collection không cần thiết phải có cùng tập các trường hay cấu trúc, và các trường thông dụng trong các document có thể chứa các kiểu dữ liệu khác nhau.
Ta có thể lập ra bảng so sánh giữa RDBMS và MongoDB để dễ liên hệ kiến thức cũ với kiến thức mới.

RDBMS	MONGODB
Database	Database
Table	Collection
Tuple/Row	Document
Column	Field
Table Join	Embedded Documents
Primary Key	Primary Key (key mặc định là _id được cung cấp bởi tự thân mongodb)
DATABASE SERVER VÀ CLIENT
Mysqld/Oracle	mongod
mysql/sqlplus	mongo
Cài đặt

Ta tiến hành cài đặt MongoDB trên Linux thông qua các bước sau:

# Download file .tgz
curl -O https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.0.4.tgz
 
# Giải nén file vừa tải về
tar -zxvf mongodb-linux-x86_64-3.0.4.tgz
 
# Copy vào một thư mục mong muốn
mkdir -p mongodb
cp -R -n mongodb-linux-x86_64-3.0.4/ mongodb
 
# Thiết lập biến môi trường để có thể thực thi lệnh của MongoDB trong môi trường dòng lệnh.
# mở file .bashrc
sudo gedit ~/.bashrc
 
# thêm dòng code thiết lập biến môi trường vào cuối file .bashrc
export PATH=<mongodb-install-directory>/bin:$PATH
 
# reset để thiết lập có hiệu lực
source ~/.bashrc
 
# tắt command line và mở lại kiểm tra
echo $PATH
 
# tạo thư mục mặc định để MongoDB lưu trữ dữ liệu
mkdir -p /data/db
 
# set permissions cho thư mục này
sudo chmod 777 -R /data
 
# nếu không muốn sử dụng thư mục mặc định này, bạn có thể thiết lập lại như sau
mongod --dbpath <đường dẫn đến thư mục chứa dữ liệu mong muốn>
Nếu cài đặt thành công, ta sẽ thấy kết qủa như bên dưới khi start dịch vụ MongoDB. Lúc này MongoDB sẽ đợi các connection kết nối vào dịch vụ đang chạy.

1
mongod
MongoDB waiting for connection
MongoDB waiting for connection
Ta mở thêm hai cửa sổ terminal và nhập dòng lệnh như bên dưới để tạo ra hai connection. Nếu thành công ta sẽ được như hình bên dưới.

1
mongo
MongoDB with 2 connections
MongoDB with 2 connections