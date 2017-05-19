---
layout: post
title:  "Mở đầu với Gulp"
date:   2017-05-20
categories: jekyll update
---
![Don’t make me think](https://viblo.asia/uploads/a8333146-6c7d-4ef7-8807-ef6d24547131.jpg)
>Mở đầu với Gulp
Gulp là một tool viết bằng Javascript, được sử dụng để tự động hoá các tác vụ giúp các bạn có thể tiết kiệm rất nhiều thời gian trong quá trình làm việc. Dù bạn có là một developer hay là một designer (người sẽ phải làm quen với HTML wireframes hiện tại hoặc sau này), tôi cũng khuyến khích hãy thử sử dụng Gulp.
Trong nội dung bài viết này, tôi sẽ đề cập đến những thao tác cơ bản với Gulp - từ cài đặt tới các ví dụ - để các bạn có thể ngay lập tức sử dụng sau khi đọc xong.
# Cài đặt Gulp
Bạn cần sử dụng Terminal ở OSX và Linux, hoặc Command Prompt của Windows.

Thử gõ npm -v và xem kết quả, nếu xuất hiện version number thì tức là bạn đã cài đặt Node từ trước rồi. Bạn bắt buộc phải có Node để cài Gulp.

Nếu kết quả là command not found (hoặc lỗi tương tự), hãy vào trang web của Node.js và thực hiện download package phù hợp với hệ thống của bạn. Sau khi cài đặt xong, bạn có thể thực hiện lại câu lệnh npm.

Để cài Gulp, bạn chỉ cần chạy câu lệnh sau:
`npm install -g gulp`
Option -g để cài Gulp trên toàn hệ thống của bạn.
# Đưa Gulp vào trong Project
Gulp đã được cài đặt từ bước trước, giờ điều chúng ta phải làm là đưa Gulp vào các project mong muốn.

Trước hết, tạo một thư mục project mới, vào trong và gõ
`npm install --save-dev gulp`

Câu lệnh trên sẽ tạo thư mục node_modules và file npm-debug.log trong thư mục dự án. Tạm thời bạn chưa cần quan tâm đến mấy cái này vội làm gì.

Lý do chúng ta phải thêm Gulp vào từng project riêng biệt, là bởi vì các project sẽ có các yêu cầu khác nhau. Có project cần SASS, có project cần Less v.vv..
# Sử dụng Gulpfile
Gulpfile là nơi để bạn định nghĩa sẽ sử dụng các automations gì và như thế nào.

Hãy tạo một file mới gulpfile.js và đưa đoạn code sau vào:
```bash
var gulp = require('gulp');

gulp.task('default', function() {

});
```
Save file lại và chạy thử Gulp trên Terminal bằng câu lệnh sau:
`gulp`
Bạn sẽ thấy kết quả hiển thị ở Terminal như sau:
```bash
gulp
[17:12:44] Using gulpfile ~/Study/nodejs/test/gulpfile.js
[17:12:44] Starting 'default'...
[17:12:44] Finished 'default' after 85 μs
```
Như vậy là bạn đã có thể chạy Gulp thành công, mặc dù chả có gì xảy ra vì bạn đang để trống phần xử lý tasks trong gulpfile.js mà. Hãy tiếp tục với ví dụ cụ thể hơn sau đây !
# Copy file
Đây là một ví dụ nhàm chán, tuy nhiên tôi lựa chọn nó vì nó dễ hiểu.

Trong thư mục project, tạo một file original.txt, và một thư mục destination. Sau đó vào Gulpfile và tạo một task copy:
```bash
gulp.task('copy', function() {
  return gulp.src('original.txt')
    .pipe(gulp.dest('destination'));
});
```
Dòng đầu tiên định nghĩa tên của task là copy. Dòng tiếp theo sử dụng gulp.src để chỉ định file chúng ta hướng tới (trong ví dụ là original.txt). Sau đó ta sử dụng pipe để thực hiện tác vụ tiếp theo: dùng gulp.dest chỉ định nơi đặt file đã xử lý.

Save file và chạy câu lệnh sau ở Terminal:
`gulp copy`
Bạn sẽ thấy file được copy vào trong thư mục destination. Log ở Terminal sẽ như sau:
```bash
gulp copy
[17:29:29] Using gulpfile ~/Study/nodejs/test/gulpfile.js
[17:29:29] Starting 'copy'...
[17:29:29] Finished 'copy' after 24 ms
```
#Theo dõi files và folders
Việc cứ phải gõ câu lệnh mỗi khi chạy một task xem chừng không hiệu quả lắm, đặc biệt với trường hợp như sự thay đổi thường xuyên ở các stylesheet files. Nhận thấy vấn đề đó, Gulp cho phép bạn có thể theo dõi sự thay đổi và chạy câu lệnh một cách tự động.
```bash
var sass = require('gulp-sass');

gulp.task('sass', function () {
    gulp.src('styles.scss')
        .pipe(sass())
        .pipe(gulp.dest('./css'));
});

gulp.task('automate', function() {
    gulp.watch('*.scss', ['sass']);
});
```
Khi bạn gõ gulp automate vào Terminal (trước đó có thể bạn phải dùng npm install gulp-sass để cài đặt plugin này), task automate sẽ start và finish, nhưng không trả lại prompt bởi vì nó đang tiếp tục theo dõi sự thay đổi ở các files chỉ định (trong ví dụ là các file có phần mở rộng là scss). Khi có file bị thay đổi, task sass sẽ được gọi để convert sang css.
```bash
gulp automate
[10:57:30] Using gulpfile ~/Study/nodejs/test/gulpfile.js
[10:57:30] Starting 'automate'...
[10:57:30] Finished 'automate' after 7.47 ms
[10:58:13] Starting 'sass'...
[10:58:13] Finished 'sass' after 12 ms
```
# Chạy multiple tasks
Có nhiều trường hợp chúng ta phải xem xét việc xử lý nhiều tasks chứ không chỉ đơn thuần một task. Có hai cách cho các bạn lựa chọn:

- Nếu những tasks đó có liên quan đến nhau, hãy sử dụng pipe để chain chúng lại.
- Nếu những tasks đó không liên quan đến nhau, hãy tham khảo ví dụ sau về việc gọi nhiều tasks trong một lần:
```bash
var gulp = require('gulp');
var uglify = require('gulp-uglify');
var concat = require('gulp-concat');
var sass = require('gulp-sass');

gulp.task('scripts', function() {
  gulp.src('js/**/*.js')
    .pipe(concat('scripts.js'))
    .pipe(gulp.dest('.'))
    .pipe(uglify())
    .pipe(gulp.dest('.'))
});

gulp.task('styles', function() {
  gulp.src('/*.scss')
    .pipe(sass())
    .pipe(gulp.dest('./css'));
});

gulp.task('default', ['scripts', 'styles']);
```
Chỉ cần gõ gulp ở Terminal (tự động gọi đến task default), task scripts sẽ được chạy, tiếp theo là task styles.
# Kết luận
Sử dụng Gulp không khó, chính thế mà nhiều người thích sử dụng Gulp hơn Grunt vì syntax khá đơn giản. Hãy nhớ những bước sau mỗi khi muốn thêm mới một automation:

- Tìm kiếm plugin
- Cài đặt plugin
- Sử dụng require với plugin đó trong Gulpfile
- Sử dụng đúng syntax
Chúc các bạn may mắn (hoho).