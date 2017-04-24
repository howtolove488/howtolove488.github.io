---
layout: post
title:  "Tăng tốc độ load web trên mobile với công nghệ AMP của Google"
date:   2017-04-24 14:02:57 +0700
categories: jekyll update
---
![Tăng tốc độ load web trên mobile với công nghệ AMP của Google](https://viblo.asia/uploads/47f50ba3-f105-403d-9f65-6b225e553974.jpg)

>Tăng tốc độ load web trên mobile với công nghệ AMP của Google

# 1.AMP là gì ?

AMP - Accelerated Mobile Page dự án mã nguồn mở được khởi động và quản lý của Google AMP là một10/2015.Một web site được xây dựng theo chuẩn AMP sẽ có tốc độ tải trang nhanh hơn gấp 4 lần và sử dụng ít dữ công nghệ để tăng tốc độ load cho website khi được truy cập qua các thiết bị di động được Google công bố đầu tiên vào tháng - là công cụ để build một web page, và render một cách nhanh nhất những nội dung cố định trên page đó. 

# 2.AMP hoạt động như thế nào?

Việc sử dụng AMP sẽ tập trung vào 3 phần chính là:
+ AMP HTML:là một tập con của HTML là một ngôn ngữ đánh dấu có một số thẻ tùy chỉnh và một số thuộc tính và một số giới hạn sử dụng

+ AMP JS: là một javascript framework cho các trang mobile.Nó quản lý cách trình bày tài nguyên và việc load tài nguyên một cách không đồng bộ.

+Google web cache - AMP CDN
tùy chọn mạng phân phối nội dung, nó sẽ lấy các trang AMP hợp lệ, cache chúng và tự động thực hiện một số công việc tối ưu load page AMP chỉ cho di động để tăng tốc load page thì phải đánh đổi rất nhiều về javascript, css hiện tại amp không cho phép các form, mã javascript xuất hiện trong trang, vì thế bạn sẽ không thể để các form , các on-page comments và một số phần tử khác mà bạn hay dùng trong một trang web thông thường.Vậy có nghĩa là để có một trang AMP hợp lệ bạn cần phải viết template cho site của bạn để nó phù hợp với các quy định của AMP. Ví dụ, tất cả CSS trong AMP phải được viết in-line và phải ít hơn 50KB, việc load custom fonts cũng cần được load sử dụng một extension amp-font đặc biệtcác tài nguyên media trên trang cũng cần được xử lý đặc biệt. Ví dụ, các ảnh cần dùng thành phần amp-imp và phải được xác định kích thước rõ ràng -width bao nhiêu, và height bao nhiêu. Thêm vào đó, nếu là ảnh động GIFs, bạn cần dùng thành phần amp-anim của AMP
Nói chung bạn sẽ phải có 2 version cho bất kỳ một trang tin nào của site: thứ nhất là phiên bản gốc của trang tin mà người dùng thường thấy ví dụ khi họ đang lướt web thông qua desktop và một AMP version cho trang đó. Để google (hay các search engine có hỗ trợ AMP) phát hiện AMP version của một page, cần thay đổi một chút cho cả phiên bản gốc và AMP version của page đó. Phiên bản gốc phải được đưa vào một tag như sau:
```bash
<link rel="amphtml" href="http://www.example.com/blog-post/amp/">
```
Còn AMP version page cần đưa vào một tag như sau
```bash
<link rel="canonical" href="https://www.example.com/url/to/full/document.html">
```
Nếu page chỉ có AMP version bạn cần thêm thẻ canonical link tới nó, đơn giản trỏ tới chính nó
```bash
<link rel="canonical" href="https://www.example.com/url/to/full/document.html">
```
Tạo trang amp page cho thapthanh.com
# 3. Cách tạo ra một trang AMP cho site

Cấu trúc chung cho một AMP template
```bash
<!doctype html>
<html ⚡>
 <head>
   <meta charset="utf-8">
   <link rel="canonical" href="hello-world.html">
   <meta name="viewport" content="width=device-width,minimum-

scale=1,initial-scale=1">
   <style amp-boilerplate>body{-webkit-animation:-....</style>
   <script async src="https://cdn.ampproject.org/v0.js"></script>
 </head>
 <body>Hello World!
 <!--HTML tags-->
 <!--AMP components-->
</body>
</html>
```

Start with the doctype <!doctype html>.Contain a top-level <html amp> tag (<html amp> is accepted as well).

Contain <head> and <body> tags (They are optional in HTML).Contain a <meta charset="utf-8"> tag as the first child of their <head> tag.

Contain a <script async src="https://cdn.ampproject.org/v0.js"></script> tag as the second child of their <head> tag (this includes and loads the AMP JS library).

Contain a <link rel="canonical" href="$SOME_URL" /> tag inside their <head> tag that points to the regular HTML version of the AMP HTML document or to itself if no such HTML version exists.

Contain a <meta name="viewport" content="width=device-width,minimum-scale=1"> tag inside their <head> tag. It's also recommended to include initial-scale=1.

Contain the following in their <head> tag: 
```bash
<style amp-boilerplate>body{-webkit-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-moz-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-ms-animation:-amp-start 8s steps(1,end) 0s 1 normal both;animation:-amp-start 8s steps(1,end) 0s 1 normal both}@-webkit-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-moz-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-ms-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-o-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}</style><noscript><style amp-boilerplate>body{-webkit-animation:none;-moz-animation:none;-ms-animation:none;animation:none}</style></noscript>
```

amp-img: dùng để thay thế img tag trong HTML
cú pháp:
```bash
<amp-img src="/img/amp.jpg" width="1080" height="610" layout="responsive" alt="an image"></amp-img>
```
Chú ý layout attribute là tùy chọn width, height là 2 thuộc tính bắt buộc và cần phải có các giá trị rõ ràng 
src phải là HTTPS

amp-video: dùng để thay thế cho video tag trong HTML5
Cú pháp: 
```bash
<amp-video width=480
 height=270
  src="/video/tokyo.mp4"
  poster="/img/tokyo.jpg"
  layout="responsive"
  controls>
  <div fallback>
    <p>Your browser doesn't support HTML5 video.</p>
  </div>
  <source type="video/mp4" src="/video/tokyo.mp4">
  <source type="video/webm" src="/video/tokyo.webm">
</amp-video>
```
Chú ý: poster attributes là ảnh được hiển thị trước khi video start, mặc định là frame đầu tiên 

src bắt buộc phải là HTTPS 

width, height là bắt buộc và phải được xác định giá trị rõ ràng
Ngoài ra còn có các thuộc tính khác như: loop, muted, autoplay

amp-anim: dùng cho ảnh động, điển hình như GIF
cú pháp:
```bash
<amp-anim width=400 height=300 src="my-gif.gif"></amp-anim>
```
Chú ý: Để dùng được amp-anim cần phải load thêm thư viện javascript sau:
```bash
<script async custom-element="amp-anim" src="https://cdn.ampproject.org/v0/amp-anim-0.1.js"></script>
```
2 thuộc tính width, height cần được xác định rõ ràng 

amp-anim còn hỗ trợ một tùy chọn placeholder child để hiển thị trong khi src file đang được load 

amp-audio: dùng để thay thế cho audio tag trong HTML5 

cú pháp: 
```bash
<amp-audio width="auto" height="50"
  src="https://ia801402.us.archive.org/16/items/EDIS-SRP-0197-06.mp3">
  <div fallback>
    <p>Your browser doesn’t support HTML5 audio</p>
  </div>
</amp-audio>
```
chú ý: amp-audio không hỗ trợ responsive layout nhưng bạn có thể có điều đó bằng việc set height là fixed và width là auto

Để dùng được amp-audio cần load thêm thư viện javascript sau:
```bash
<script async custom-element="amp-audio" src="https://cdn.ampproject.org/v0/amp-audio-0.1.js"></script>
```
amp-carousel: được dùng cho slide show 

cú pháp: 
```bash
<amp-carousel width=300 height=400>
  <amp-img src="my-img1.png" width=300 height=400></amp-img>
  <amp-img src="my-img2.png" width=300 height=400></amp-img>
  <amp-img src="my-img3.png" width=300 height=400></amp-img>
</amp-carousel>
```
chú ý: Để dùng được amp-carousel cần thêm thư viện javascript sau: 
```bash
<script async custom-element="amp-carousel" src="https://cdn.ampproject.org/v0/amp-carousel-0.1.js"></script>
```
Các thuộc tính khác như: type (carousel hoặc slides), layout, control, autoplay, delay 

amp-iframe: dùng để hiển thị một iframe 

cú pháp:
```bash
<amp-iframe width=300 height=300
    sandbox="allow-scripts allow-same-origin"
    layout="responsive"
    frameborder="0"
    src="https://foo.com/iframe">
</amp-iframe>
```
Chú ý:Để dùng được amp-iframe cần đưa vào thư viện javascript sau:
```bash
<script async custom-element="amp-iframe" src="https://cdn.ampproject.org/v0/amp-iframe-0.1.js"></script>
```
amp-iframe không được xuất hiện sát trên cùng của page ngoại trừ iframe đó dùng placeholder. Nó phải cách top của page ít nhất 600px hoặc không chiếm quá 75% viewport 

src của amp-iframe yêu cầu phải là HTTPS

amp-font: Điều khiển việc load custom font trên các AMP pages 

cú pháp: 
```bash
<amp-font
    layout="nodisplay"
    font-family="Comic AMP Bold"
    timeout="3000"
    font-weight="bold"
    on-error-remove-class="comic-amp-bold-font-loading"
    on-error-add-class="comic-amp-bold-font-missing"
    on-load-remove-class="comic-amp-bold-font-loading"
    on-load-add-class="comic-amp-bold-font-loaded">
</amp-font>
```
Chú ý:
Để dùng được amp-font cần đưa vào thư viện javascript sau:
```bash
<script async custom-element="amp-font" src="https://cdn.ampproject.org/v0/amp-font-0.1.js"></script>
```
amp-youtube: Để dùng cho việc nhúng video youtube
cú pháp: 
```bash
<amp-youtube data-videoid="mGENRKrdoGY" layout="responsive" width="480" height="270"></amp-youtube>
```
chú ý: 

Để dùng được amp-youtube cần thêm thư viện javascript sau:
```bash
<script async custom-element="amp-youtube" src="https://cdn.ampproject.org/v0/amp-youtube-0.1.js"></script>
```
data-videoid có giá trị chính là youtube id có thể xác định được từ url của youtube 

Ngoài ra còn rất nhiều component khác mà AMP hỗ trợ. Bạn có thể tham khảo thêm trên trang: https://www.ampproject.org/ 

# 4. Cách validate một trang amp

Các bước cần thực hiện để validate một AMP page:
+ mở page trong trình duyệt Chrome
+ Thêm "#development=1" to URL
+ Mở Chrome DevTools console và kiểm tra lỗi : click chuột chọn inspect element → sau đó chọn mục console hoặc dễ hơn bạn nhấn tổ hợp phím Ctrl + Shift + i (Windows) / Cmd + Opt + i (Mac)

![](https://viblo.asia/uploads/85e6e478-4382-489d-a935-a038adac51db.png)

Hình ảnh Chrome console với một số errors của AMP page
![](https://viblo.asia/uploads/7483ebbc-f5b1-4c85-b718-b1f41ec97a7c.png)
Hình ảnh Chrome console trên một trang AMP hợp lệ

# 5. Kết luận

AMP là một công nghệ mới được Google chính thức công bố gần đây. Đã cố rất nhiều website lớn đã áp dụng công nghệ này. Có thể nói hiệu quả marketing của nó mang lại là rất lớn. 

Bài viết đã giới thiệu tổng quan về đặc điểm, lợi ích, cách áp dụng cũng như cách kiểm tra sự hợp lệ của một trang AMP. Mong rằng đây sẽ là một tài liệu hữu ích cho bạn khi bắt đầu tìm hiểu về AMP. Cảm ơn các bạn đã quan tâm tới bài viết của tôi. Hẹn gặp lại ở các bài viết tiếp theo.
