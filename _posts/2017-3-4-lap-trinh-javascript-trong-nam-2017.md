![Lập trình Javascript trong năm 2017](https://thefullfool.files.wordpress.com/2010/09/wheres-waldo1.jpg)

> Lập trình Javascript trong năm 2017

Bạn là một lập trình viên có kinh nghiệm 5 năm làm việc với các REST API? Bạn đã tối ưu tìm kiếm cho hệ cơ sở dữ liệu khổng lồ ở công ty bạn? Bạn đã viết các chương trình nhúng cho cái lò vi sóng? Hẳn là cũng lâu rồi kể từ lúc bạn đánh vật với mấy cái Prototype.js để viết OOP phía trình duyệt sao cho chuẩn. Và bây giờ bạn quyết định rằng đã đến lúc để nâng cấp các kĩ năng front-end của bạn. Tuy nhiên khi bạn bắt đầu tìm hiểu thì bạn ngay lập tức cảm thấy như thế này.

Cái cảm giác vô định, bối rối này thực ra lại là một chuyện thường ngày ở huyện trong cộng đồng Javascript. Nếu vẫn chưa đủ khiến các bạn bối rối, thì đây là 1 cuộc nói chuyện giữa 2 người bạn, phản ánh chính xác tình trạng trên.

Nhưng bạn lại không có thời gian để mà lo lắng về 1 đống công nghệ trôi nổi xung quanh ấy. Bạn hiện giờ đang ở trong một cái mê cung, và bạn cần 1 cái bản đồ. Được thôi, tôi sẽ làm cho bạn 1 cái.

Tuy nhiên có 1 chút lưu ý nho nhỏ: đây là 1 tài liệu tham khảo giúp bạn nhanh chóng sử dụng các công nghệ hiện thời mà không phải suy nghĩ quá nhiều. Về cơ bản tôi sẽ phác ra 1 bản danh sách các công cụ kết hợp tốt với nhau cho nhu cầu thường thấy của các lập trình viên front-end. Điều này sẽ giúp bạn thoải mái, tránh đi những cơn đau đầu không cần thiết. Một khi bạn đã hoàn thành các topic này, bạn có thể tự tin tiếp tục với stack mà bạn thấy thích thú.

Làm quen và triển khai các kiến trúc xây dựng ứng dụng trong khóa thực tập NodeJS Full Stack

Cấu trúc "bản đồ"

Tôi sẽ chia thành các vấn đề nhỏ để bạn có thể tập trung xử lý từng cái một. Với mỗi vấn đề đó, tôi sẽ:

Mô tả nhu cầu sử dụng công cụ.
Quyết định sử dụng công cụ nào đối với vấn đề đó.
Giải thích tại sao tôi lại chọn nó.
Đưa ra một số giải pháp thay thế.
Quản lý package

Vấn đề: cần thiết trong việc tổ chức project và các dependency.
Giải pháp: NPM và Yarn.
Lý do chọn: hẳn bạn chẳng lạ gì NPM, trình quản lý package thông dụng nhất hiện nay. Yarn, hướng đến giải quyết các vấn đề về tối ưu các dependency, kiểm soát phiên bản của các thư viện bạn sử dụng trong 1 file lock (sử dụng song song với phiên bản ngữ nghĩa - semantic version của NPM, theo cách bổ sung cho nhau).
Một số phương án thay thế: trong giới hạn kiến thức của tôi thì là không có.
Javascript

Vấn đề: ECMAScript 5 (hay còn được gọi là anh già Javascript) tồi quá.
Giải pháp: ES6.
Lý do chọn: Kết hợp rất nhiều tính năng có sẵn trên các ngôn ngữ lập trình khác. Vài tính năng thú vị như: arrow function, module import - export, de-structuring, template string, let và const, generator, promise. Nếu là một lập trình viên Python thì bạn sẽ thoải mái thôi.
Một số phương án thay thế: TypeScript, CoffeeScript, PureScript, Elm.
Biên dịch

Vấn đề: Nhiều trình duyệt đang được sử dụng vẫn chưa hỗ trợ ES6. Bạn sẽ cần 1 trình để biên dịch ES6 sang các chuẩn thấp hơn để tương thích với các trình duyệt, như là ES5.
Giải pháp: babel.
Lý do chọn: làm việc rất hoàn hảo. Biên dịch phía server.
Một số phương án thay thế: Traceur.
Lưu ý: sử dụng babel-loader, 1 loader Webpack. 
Linting (tìm lỗi code trong nguồn mà không cần phải chạy code)

Vấn đề: có rất nhiều cách viết mã Javascript và điều đó dẫn đến việc rất khó đảm bảo consistency (tính chắc chắn). Các linter có thể ngăn chặn được một số bug.
Giải pháp: ESLint.
Lý do chọn: code rất tuyệt, dễ config. Airbnb preset là mọi thứ bạn cần có để thiết lập và chạy. Giúp bạn làm quen với cú pháp mới.
Một số phương án thay thế: JSLint.
Bundling (đóng gói code)

Vấn đề: số lượng file trong project ngày càng nhiều. Các dependency cần được xử lý và gọi một cách đúng đắn.
Giải pháp: Webpack
Lý do chọn: dễ cấu hình. Có thể load tất cả các loại dependency và asset. Tính tương thích cao. Là bundler được ưa thích cho các project React.
Một số phương án thay thế: Browserify.
Nhược điểm: cấu hình lần đầu có 1 chút khó khăn.
Lưu ý: nếu bạn muốn dành thời gian để hiểu thêm về bundler này, bạn cần học về babel-loader, style-loader, css-loader, file-loader, url-loader.
Test

Vấn đề: ứng dụng của bạn không hoàn hảo. Với rất nhiều bug (chắc chắn 😎) thì nó sẽ dễ dàng xảy ra vấn đề, hỏng hóc. Bạn cần test để tránh tối đa lỗi có thể xảy ra.
Giải pháp: mocha (test runner), chai (thư viện assertion), và chai-spies (cho các spy, fake object mà bạn có thể truy xuất với bất cứ sự kiện nào - mong muốn hay không mong muốn xảy ra). 
Lý do chọn: sử dụng rất đơn giản, rất mạnh mẽ.
Một số phương án thay thế: Jasmine, Jest, Sinon, Tape.
Framework UI/ quản lý trạng thái

Vấn đề: Các SPA (Single Page Application) đang ngày càng nhiều, càng phức tạp. Do đó các state (trạng thái) hay thay đổi trở thành rắc rối lớn.
Giải pháp: React và Redux.
Lý do chọn React: Tạo sự thay đổi lớn trong cách tư duy, xóa bỏ nhiều ràng buộc cũ kĩ của các website, làm cho các website trở nên tuyệt vời hơn. Không còn sử dụng phương pháp truyền thống nữa, thay vào đó là chia nhỏ các mối quan tâm: bỏ đi cách chia các thành phần theo công nghệ (HTML, CSS, JS), chuyển sang cách chia tách chúng theo chức năng (các component). UI lúc này hoàn toàn là các function thuần của state. 
Lý do chọn Redux: Nếu bạn có ý định xây dựng 1 ứng dụng lớn, bạn cần 1 công cụ để quản lý state (mặt khác bạn sẽ cần các component tương tác với nhau, hãy học Vanilla inter-component communication trước để trải nghiệm). Redux sẽ thực thi pattern theo 1 cách đơn giản. Facebook cũng sử dụng Redux. Thêm vào đó là rất nhiều điều tuyệt vời: reload và vẫn giữ nguyên các state, luồng dữ liệu, các test.
Một số phương án thay thế: Angular2, Vue.js.
Cảnh báo: Lần đầu tiên nhìn thấy JSX hẳn là bạn đã rất bực mình. Chắc hẳn bạn đã phải kháng cự lại mong muốn tìm ngay một forum và gào thét lên với các đồng đạo. Nó chỉ là do sự mâu thuẫn trong nhận thức. Sau khi học 1 thời gian, bạn mới nhận ra rằng trộn lẫn HTML, Javascript, CSS trong 1 file đơn lẻ hóa ra cũng không phải là 1 ý tưởng tồi tệ. 
Thao tác, animate DOM

Vấn đề: Bạn thỉnh thoảng vẫn cần quick fix mỗi khi bạn chọn lựa các selector và thực hiện các hoạt động trực tiếp trên các nút DOM.
Giải pháp: ES6 hay jQuery.
Lý do chọn: Hiển nhiên là jQuery vẫn đang sống mạnh và khỏe. React và jQuery chả đụng chạm gì đến nhau cả. Tuy nhiên, có khả năng là phần lớn bạn sẽ làm việc với vanilla React (và querySelector). Và khi đó, việc thêm jQuery sẽ làm tăng thêm rắc rối cho bundle. Sử dụng jQuery ở tầng trên React thực sự không ổn tí nào, bạn cần lưu ý tránh điều đó. Nếu bạn đang đụng phải 1 vấn đề mà bạn không biết xử lý thế nào với React và ES6, hay như các vấn đề về cross browser, thì có thể jQuery sẽ hữu dụng.
Một số phương án thay thế: Dojo (tôi không biết là nó có còn tồn tại hay không nữa?).
Styling

Vấn đề: Bạn đang có các module thích hợp, bạn muốn chúng là những mảnh hoạt động độc lập, có thể tái sử dụng. Khi đó style của component cần phải linh động như chính component đó.
Giải pháp: CSS module.
Lý do chọn: Mặc dù tôi thích dùng inline-style (và sử dụng chúng rất nhiều), tôi vẫn phải thừa nhận rằng chúng khá là giới hạn. Đúng là hoàn toàn được khi sử dụng inline-style trong React, tuy nhiên bạn sẽ không thể sử dụng được các giả-selector (như là :hover) với chúng. Điều đó sẽ phá tan tành ứng dụng của bạn.
Một số phương án thay thế: Inline style. Điều tôi thực sự khoái inline-style trong React là nó cho phép bạn làm việc với các style như là các object Javascript thông thường. Inline-style cũng ở trong cùng file với component, điều này rất tốt cho quá trình maintain. Một vài người vẫn đang ủng hộ cho SASS, SCSS hay LESS. Tuy nhiên chúng yêu cầu phải thông 1 bước để build, cũng như không linh hoạt như CSS module hay là inline-style.
Vậy là đủ!

Đến giờ thì bạn đã có đống thứ linh tinh để học, nhưng ít nhất thì bạn không cần mò mẫm nghiên cứu nữa. Tôi có quên cái gì không nhỉ? Hay là tôi có sai gì không? Hãy để lại nhận xét hay liên lạc với tôi qua twitter nhé. 

Bài viết được dịch từ: https://hackernoon.com/a-map-to-modern-javascript-development-2017-16d9eb86309c#.xydy1ve69