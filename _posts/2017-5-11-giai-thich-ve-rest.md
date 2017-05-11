---
layout: post
title:  "Giải thích về Rest"
date:   2017-05-11
categories: jekyll update
---
![React with graphql](https://www.api2cart.com/wp-content/uploads/2015/04/rest-api.png)
>REST là kiến trúc phần mềm phổ biến nhất hiện nay trên internet. Thực tế khi đọc bài viết về REST các bạn sẽ thấy nó hơi bị mơ hồ khó hiểu.

# REST là gì?
Những khái niệm đầu tiên về REST(REpresentational State Transfer) được đưa ra vào năm 2000 trong luận văn tiến sĩ của Roy Thomas Fielding (đồng sáng lập giao thức HTTP). Trong luận văn ông giới thiệu khá chi tiết về các ràng buộc, quy ước cũng như cách thức thực hiện với hệ thống để có được một hệ thống REST.

Hiểu đơn giản nó là một bộ các ràng buộc và quy ước , khi áp dụng đầy đủ vào hệ thống của bạn thì ta có 1 hệ thống REST.

REST Contraints
- Hệ thống hoạt động theo mô hình client-server, trong đó server là tập hợp các service nhỏ lắng nghe các request từ client. Với từng request khác nhau thì có thể một hoặc nhiều service xử lý.

- Stateless (phi trạng thái). Đơn giản server và client không lưu trạng thái của nhau -> mỗi request lên server thì client phải đóng gói thông tin đầy đủ để thằng server hiểu được. Điều này giúp hệ thống của bạn dễ phát triển,bảo trì, mở rộng vì không cần tốn công CRUD trạng thái của client . Hệ thống phát triển theo hướng này có ưu điểm nhưng cũng có khuyết điểm là gia tăng lượng thông tin cần truyền tải giữa client và server.

- Khả năng caching : Các response có thể lấy ra từ cache. Bằng cách cache các response , server giảm tải việc xử lý request, còn client cũng nhận được thông tin nhanh hơn. Ở đây ta đặt 1 thằng cache vào giữa : client- cache- server.

- Chuẩn hóa các interface : Đây là một trong những đặc tính quan trọng của hệ thống REST. Bằng cách tạo ra các quy ước chuẩn để giao tiếp giữa các thành phần trong hệ thống, bạn đã đơn giản hóa việc client có thể tương tác với server. Các quy ước này áp dụng cho toàn bộ các service giúp cho người sử dụng hệ thống của bạn dễ dụng hơn. Dễ hiểu hơn trên hệ thống bạn đặt ra 1 chuẩn API để người dùng dù là mobile, web đều có thể kết nối vào được. Hệ thống REST có yếu điểm ở đây vì khi chuẩn hóa rồi ta không thế tối ưu từng kết nối.

- Phân lớp hệ thống : trong hệ thống REST bạn chia tách các thành phần hệ thống theo từng lớp, mỗi lớp chỉ sử dụng lớp ở dưới nó và giao tiếp với lớp ở ngay trên nó mà thôi. Điều này giúp bạn giảm độ phức tạp của hệ thống,giúp các thành phần tách biệt nhau từ đó dễ dàng mở rộng từng thành phần :
![React with graphql](https://viblo.asia/uploads/64845983-c416-453f-8943-1a94d9aef445.png)

# Resources (Tài nguyên)
Hệ thống REST trước hệt phải tuân thủ các ràng buộc ở trên. Đi vào chi tiết, hệ thống REST tập trung vào việc xử lý các tài nguyên. Resource là bất cứ cái gì mà bạn có thể gọi tên được ( một video, ảnh, trang web, báo cáo thời tiết .v.v). Các tài nguyên này giúp ta định nghĩa được các services trong hệ thống, kiểu thông tin mà nó trả về, và hành vi xử lý thông tin của nó.
Các đặc tính mô tả một tài nguyên :
- Nhiều cách thức hiển thị : dữ liệu bạn nhận được có thể ở nhiều dạng ( binary, JSON, XML .v.v) dữ liệu này đại diện cho 1 tài nguyên xác định.

- Nhận diện rõ ràng : Mỗi URL tại một thời điểm chỉ trả về 1 tài nguyên xác định.

- Dữ liệu mô tả (metadata) : Kiểu nội dung( Content-type), lần cập nhâp mới .v.v

- Dữ liệu điều khiển : Is-modifiable-since, cache-control.
Thông thương nhắc đến REST là nhắc đến HTTP vì hệ thống REST thường sử dụng giao thức HTTP. Hệ thống REST xoay quanh viêc đơn giản hóa việc lấy các representation của một tài nguyên hệ thống.

Nhận diện tài nguyên : hệ thống cần cung cấp giải pháp để nhận diện được các tài nguyên trong hệ thống, thông thường nó dùng đường dẫn tuyệt đối tới tài nguyên đó. Mỗi route cho ta dữ liệu xác định về các đối tượng cụ thể:
`GET /api/v1/books/last`
Đây là giải pháp không theo REST vì cái route này trỏ đến nhiều thằng khác nhau theo thời gian. Với REST với mỗi route là tài nguyên xác định mà thôi
`GET /api/v1/books/harry-potter-and-the-deathly-hollows`
Vậy làm sao ta có được dữ liệu cuốn sách gần nhất, hãy sử dụng các query string
`GET /api/v1/books?limit=1&sort=created_at`
Có nhiều tranh luận về việc không sử dụng id trong REST, nghĩa là bạn không nên lấy một giá trị số (id của dữ liệu trong cơ sở dữ liệu chẳng hạn ) đại diện cho một resource vì nó mất đi tính đại diện cho resource đó . Ví dụ
`GET /api/v1/books/j-k-rowling/harry-potter-and-the-deathly-hollows`
Route của bạn khá rõ ràng còn nếu bạn dùng ID thì
`GET /api/v1/books/55/3`
Tùy biến hiển thị dữ liệu : Như các bạn biết một resource có nhiều cách thức hiển thị vậy làm sao để người dùng có thể lựa chọn được nó. Có 2 giải pháp:
- Content Negotiation - Nhét các yêu cầu vào trong header nếu dùng HTTP ví dụ "Accept: text/html;"
- Sử dụng mở rộng file
```bash
GET /api/v1/books .json
GET /api/v1/books .xml
```
Rõ ràng cách 2 trông có vẻ là dễ sư dụng hơn nhưng nó khó tùy biến hơn so với cách 1.

Chuẩn hóa tương tác với tài nguyên. Chúng ta đã xác định được vị trí tài nguyên, chúng ta cần tương tác với nó CRUD. Từ khi REST dử dụng HTTP như chuẩn khởi tạo hệ thống, nó giới hạn một số các method để tương tác với tài nguyên. Chúng ta sử dụng GET để truy xuất đọc dữ liệu, POST để đưa dữ liệu mới lên server. PUT để cập nhật dữ liệu và DELETE để xóa dữ liệu. Đôi khi POST và PUT hoán đổi cho nhau. Bạn có thể đánh giá cấp độ sử dụng REST trong hệ thống theo hình sau :
![React with graphql](https://viblo.asia/uploads/d1cbcd87-89d5-41e7-b3eb-45d3be510938.png)
Rõ ràng hơn với REST:
```bash
PUT /api/v1/blogposts/12342 ?action=like
GET /api/v1/books ?q=[SEARCH-TERM]
GET /api/v1/authors ?filters=[COMMA SEPARATED LIST OF FILTERS]
```
- Dữ liệu mô tả : Để có thể thực hiện ràng buộc về chuẩn hóa giao tiếp. Dữ liệu trả về của hệ thống REST cần chưa các thông tin về thằng trả dữ liệu về nữa. Nó tuân thủ theo một cài cái chuẩn như HATEOAS (s Hypermedia as the Engine of Application State). Hiểu đơn giản là hệ thống của bạn gồm nhiều services làm sao để thằng client biết nó đang tương tác với thằng nào để lần sau nó tương tác trực tiếp với thằng đó thôi, bên cạnh đó nó cũng cần mô tả rõ loại dữ liệu , cấu trúc lấy dữ liệu nữa . Ví dụ :
```bash
{
 "metadata": {
   "links": [
     "books": {
       "uri": "/books",
       "content-type": "application/json"
     },
     "authors": {
       "uri": "/authors",
       "content-type": "application/json"
     }
   ]
 }
}
```
phần này giúp cho dữ liệu bạn nhận được dễ hiểu hơn và giúp client thuận tiện có thông tin để tiếp tục tương tác. Dữ liệu metadata có thể bao gồm đầy đủ các mô tả về action nâng cao, hoặc các thuộc tính hỗ trợ trong hệ thống. từ đó giúp client có thể dễ dàng tùy biến dữ liệu . Ví dụphần này giúp cho dữ liệu bạn nhận được dễ hiểu hơn và giúp thằng client thuận tiện có thông tin để tiếp tục tương tác. Dữ liệu metadata có thể bao gồm đầy đủ các mô tả về action nâng cao, hoặc các thuộc tính hỗ trợ trong hệ thống. từ đó giúp client có thể dễ dàng tùy biến dữ liệu . Ví dụ:
```bash
"links": {
   "next": {
     "uri": "/books?page=1",
     "content-type": "application/json"
   }
 }
```