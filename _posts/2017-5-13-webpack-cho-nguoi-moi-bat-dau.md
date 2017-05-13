---
layout: post
title:  "Webpack cho người mới bắt đầu"
date:   2017-05-13
categories: jekyll update
---
![Webpack cho người mới bắt đầu](https://www.api2cart.com/wp-content/uploads/2015/04/rest-api.png)
Ngày nay các website đang có xu hướng trở thành những web app với các đặc tính như:

- Càng ngày càng sử dụng JS nhiều hơn
- Những browser ngày càng hỗ trợ những công nghệ mới
- Những trang full-page-reload ít đi, single page app lên ngôi

Dẫn đến phần code client-side ngày càng nhiều. Điều đó có nghĩa chúng ta cần phải có một công cụ để quản lí chúng một cách hiệu quả. Và webpack là một công cụ rất mạnh để làm điều đó. Nó là một module bundler rất mới nhưng đã gây sốt gần đây. Nó nhận vào các module cùng với các dependencies và generate ra các static assets tương ứng.
![](https://viblo.asia/uploads/cd0b337b-c7e0-42a5-844d-72438d6b811b.png)

# Mục tiêu của Webpack
- Chia các cây dependency thành các chunk được load khi cần thiết
- Thời gian init ngắn hơn
- Mỗi static asset đểu có thể trở thành một module
- Khả năng tích hợp 3rd-party library như module
- Khả năng custom gần như mọi thành phần của module bundler
- Phù hợp với các dự án lớn

Để bắt đầu, chúng ta sẽ cài webpack bằng lệnh sau:
`npm install webpack -g`
Sau đó, tạo ra 2 file: index.html và app.js

app.js:
```bash
document.write('welcome to my app');
console.log('app loaded');
```
index.html
```bash
<html>
  <body>
    <script src="bundle.js"></script>
  </body>
</html>
```

Trong terminal, chúng ta chạy lệnh sau:
`webpack ./app.js bundle.js`
Lệnh trên webpack sẽ đọc vào file app.js và xuất ra file bundle.js cho chúng ta. Khá dễ dàng phải ko ?

# Config
Config file trong webpack là nơi chưa tất cả các thiết lập, loader, plugin và các thông số khác về build mà bạn muốn xây dựng. Bạn hãy tạo file webpack.config.js trong thư mục gốc và thêm những dòng lệnh sau:
```bash
module.exports = {
  entry: "./app.js",
  output: {
    filename: "bundle.js"
  }
}
```
entry - tên của file hoặc một mảng những file mà chúng ta muốn include. Trong ví dụ này, chúng ta dùng 1 file duy nhất là app.js.

output - là một object chứa những thiết lập output. Trong ví dụ này, chúng ta chỉ ra filename là bundle.js là file mà chúng ta muốn Webpack build ra.

Bây giờ chúng ta hãy quay trở lại Terminal và gõ lệnh sau:
`webpack`
Khi tồn tại file webpack.config.js, webpack sẽ build dựa vào file config đó, mà không cần chúng ta phải chỉ rõ ra file input và file output mỗi khi gõ lệnh như ví dụ trước đó.

# Watchmode
Hẳn mỗi lần bạn sửa file, bạn không muốn lại phải vào Terminal và chạy lại webpack để build ra file kết quả bạn mong muốn. Watchmode sẽ giúp bạn theo dõi files, và khi chúng có bất kì thay đổi nào, nó sẽ ngay lập tức build và xuất ra file output.

Có 2 cách để chạy watchmode.
- 1.Từ command line:
`webpack --watch`
- 2.Vào trong file config, thêm một key là watch và đặt nó là true
```bash
module.exports = {
  entry: "./app.js",
  output: {
    filename: "bundle.js"
  },
  watch: true
}
```
Sau khi đặt watch thành true, mỗi khi bạn chạy webpack, nó sẽ tự động rebuild file bulde mỗi khi có thay đổi.

# Webpack loader và preloader
Loader cho phép bạn tiền xử lí một file khi bạn require hoặc load nó. Loader co1 thể transform file từ nhiều ngôn ngữ khác nhau như CoffeeScript sang Javascript. Loader thậm chí còn cho phép bạn require css file ngay trong Javascript.

Sau đây, chúng ta sẽ cài đặt và sử dụng loader babel. Trước khi có thể dùng được, chúng ta cần phải cài một số npm module trước.
`npm install babel-core babel-loader babel-preset-es2015 babel-preset-react webpack  --save-dev`
- babel-core là package babel của npm
- babel-loader là loader của babel cho webpack
- babel-preset-es2015 là babel preset cho tất cả plugin của ES2015
- babel-preset-react giúp transform code JSX của React thành Js
- --save-dev lưu tất cả các module thành development dependencies.
Bây giờ để cài dặt babel transform code ES6, chúng ta cần chỉnh sửa webpack.config.js một chút. Đầu tiên là thêm một key là module,trong đó thêm mảng loaders, mỗi phần tử của mảng là config cho một loader. Sau đây chúng ta sẽ config cho babel-loader.
```bash
module.exports = {
 entry: "./app.js",
 output: {
   filename: "bundle.js"
 },
 module: {
   loaders: [
     {
       test: /\.js$/,
       exclude: /node_modules/,
       loader: 'babel-loader',
       query: {
         presets: ['react', 'es2015']
       }
     }
   ]
 },
 resolve: {
   extensions: ['', '.js']
 },
}
```
Như các bạn có thể thấy ở trên, chúng ta thêm 3 key vào trong loader đầu tiên

- 1.test - là một regular expression dùng để kiểm tra những file nào sẽ được chạy loader. Trong ví dụ này, tất cả những file có extension là .js sẽ được chạy babel-loader.

- 2.exclude - những file mà loader sẽ loại bỏ (ignore). Chúng ta thêm node_modules vào đây.

- 3.loader - tên của loader chúng ta dủng (babel-loader)

- 4.query - Bạn có thể truyền options cho loader bằng cách viết chúng như những query string.

- 5.cacheDirectory - mặt định false. Khi được đặt, thư mục được chọn sẽ được sử dụng để cache kết quả từ loader.

- 6.presets chúng ta sẽ sử dụng react và es2015 đã được cài đặt

Chúng ta còn thêm key resolve dùng để chỉ ra những file nào có thể được xử lí mà không cần phải chỉ ra file extension. Giúp chúng ta có thể viết như này:
`require('./logger');`
thay vì
`require('./logger.js');`
Giờ dự án của bạn đã sẵn sàng để viết code ES6 và React, Webpack sẽ làm phần việc còn lại.

Hi vọng với bài hướng dẫn này các bạn sẽ có một bước khởi đầu tốt với công cụ rất mạnh như Webpack.