---
layout: post
title:  "Những khái niệm cơ bản về DOM"
date:   2017-05-12
categories: jekyll update
---
![React with graphql](https://www.api2cart.com/wp-content/uploads/2015/04/rest-api.png)

# HTML là gì?
HTML đã quá quen với lập trình viên, vậy nó được hiểu như thế nào? Như các bạn đã biết HTML là ngôn ngữ đánh dấu siêu văn bản, nó là một XML namespace, hay được hiểu là tập các thẻ XML mà trình duyệt nào cũng có thể đọc được. Chúng ta nhìn vào một file HTML thì nhìn thấy text, còn trình duyệt nhìn vào sẽ thấy cây DOM.
# DOM là gì?
Thêm một khái niệm nữa, chúng ta thường nghe đến DOM và làm việc với chúng, vậy chúng được hiểu như thế nào?

![React with graphql](https://viblo.asia/uploads/54aa92df-82e9-479b-924f-4639143bda82.gif)
Chúng ta có thể thấy tất cả các thẻ HTML sẽ được quản lý trong đối tượng document (DOM), thẻ cao nhất là thẻ html, tiếp đến là phân nhánh body và head. Bên trong head thì có những thẻ như style, title,... và bên trong body chứa bất kì một thẻ nào đó là thành phần của HTML. Như vậy ta có thể hiểu trong Javascript để thao tác được với các thẻ HTML ta phải thông qua đối tượng documnent (DOM).

Với DOM, JavaScript được tất cả sức mạnh cần thiết để tạo ra HTML động:
- JavaScript có thể thay đổi tất cả các phần tử HTML trong trang
- JavaScript có thể thay đổi tất cả các thuộc tính HTML trong trang
- JavaScript có thể thay đổi tất cả các phong cách CSS trong trang
- JavaScript có thể loại bỏ các yếu tố HTML và thuộc tính hiện tại
- JavaScript có thể thêm các yếu tố HTML mới và các thuộc tính
- JavaScript có thể phản ứng với tất cả các sự kiện HTML hiện trong trang
- JavaScript có thể tạo ra các sự kiện HTML mới trong trang
Document Object Model - DOM ("Mô hình Đối tượng Tài liệu"), là một giao diện lập trình ứng dụng (API). DOM được dùng để truy xuất các tài liệu dạng HTML và XML, có dạng một cây cấu trúc dữ liệu, và thông thường mô hình DOM độc lập với hệ điều hành và dựa theo kỹ thuật lập trình hướng đối tượng để mô tả tài liệu.

Thời kì sơ khai các thành phần trong một tài liệu HTML mô tả bằng các phiên bản khác nhau của DOM được hiển thị bởi các chương trình duyệt web thông qua JavaScript vì chưa có một chuẩn thống nhất nào. Điều này buộc World Wide Web Consortium (W3C) phải đưa ra một loạt các mô tả kĩ thuật về tiêu chuẩn cho DOM để thống nhất mô hình này.
```bash
<!DOCTYPE html>
<html>
<body>

<p id="demo">Click the button to display the cookies associated with this document.</p>

<button onclick="myFunction()">Try it</button>

<script>
function myFunction() {
    document.getElementById("demo").innerHTML =
    "Cookies associated with this document: " + document.cookie;
}
</script>

</body>
</html>
```
kết quả của ví dụ trên như sau:
![React with graphql](https://viblo.asia/uploads/b1db36e0-6494-4a55-9ea6-5ceacdd85010.png)

# HTML DOM là gì?
HTML DOM là một chuẩn mô hình object và programming interface cho HTML. nó định nghĩa:
- HTML elements như là objects
- properties của tất cả HTML elements
- methods để truy cập đến tất cả HTML elements
- events cho tất cả HTML elements
HTML DOM là một tiêu chuẩn cho phép bạn thực hiện những công việc thao tác với bất kì một trang web: get, change, add, or delete các thành phần của HTML.
# DOM Attributes
Attributes property là một khái niệm của DOM trả về một tập hợp các thuộc tính của nút được chỉ định, như một đối tượng NamedNodeMap. Các nút có thể được truy cập bởi các con số chỉ số, và chỉ số bắt đầu từ 0. Và bằng số chỉ mục là hữu ích cho đi qua tất cả các thành phần của Attributes: Bạn có thể sử dụng các property của đối tượng NamedNodeMap để xác định số lượng các thuộc tính, lặp qua tất cả sau đó bạn có thể tính các nút và trích xuất các thông tin mà bạn muốn.

Xét ví dụ sau:
```bash
<!DOCTYPE html>
<html>
<body>

<p>Click the button to display the name of the button's second attribute (index 1).</p>

<button id="myBtn" onclick="myFunction()">Try it</button>

<p id="demo"></p>

<script>
function myFunction() {
    var x = document.getElementById("myBtn").attributes[1].name;
    document.getElementById("demo").innerHTML = x;
}
</script>

</body>
</html>
```
Kết quả sẽ như sau:
![try](https://viblo.asia/uploads/7d83666b-4be9-42d4-b74c-3bde8d321d61.png)
Nói tóm lại, attribute là thuộc tính của các phần tử DOM. Attribute cho biết các đặc điểm của phần tử DOM đó.
# Property
Property cung cấp thêm thông tin về các thành phần trong HTML, các phần tử DOM được ánh xạ thành các đối tượng Javascript khi ta sử dụng Javascript để thao tác với DOM.
```bash
var paragraphs = document.getElementsByTagName('P'); // (1)
var firstParagraph = paragraphs[0]; // (2)
```
Phần tử <P> đầu tiên của document được ánh xạ thành đối tượng Javascript được trỏ bởi biến firstParagraph, (getElementsByTagName() trả về một cấu trúc dữ liệu tương tự 1 mảng các Node được gọi là NodeList, tức là có thuộc tính length, và truy xuất thông qua chỉ số). Phần tử DOM được ánh xạ thành đối tượng có thuộc tính và phương thức trong Javascript. Thuộc tính của đối tượng Javascript, được gọi là property. Chung quy lại thì:
>Attribute là thuộc tính các phần tử DOM còn Property là thuộc tính của đối tượng Javascript.
**Một vài chú ý nhỏ
- Attribute của DOM element và property của Javascript object tương ứng thì không có quan hệ 1 - 1. Chẳng hạn như attribute class được ánh xạ thành property className và attribute for được ánh xạ thành htmlFor
- Dùng phương thức getAttribute(name) và setAttribute('name', 'value'). Để thao tác với property để tương tác với attribute, dùng dot notation (element.property = value)
- Attribute value của phần tử <input type="text" value="type to search"/> chính vì vậy thay đổi property không chắc chắn làm thay đổi attribute và ngược lại.
Nói một cách khái quát thì nếu giá trị trong input được định nghĩa là 'type to search', thì propery tương ứng cũng như vậy. Sau khi người dùng nhập dữ liệu, 'abc' chẳng hạn, thì property sẽ được thiết lập thành 'abc', tuy vậy, attribute vẫn không thay đổi.
```bash
console.log(input.getAttribute('value'));
// type to search

console.log(input.value);
// 'abc'
```
Mặc dù nghĩa dịch sang tiếng việt giống nhau nhưng attribute và property thuộc về 2 thế giới hoàn toàn khác nhau. Cần nắm rõ để tránh các hiểu lầm không cần thiết.

# Cây cấu trúc trong DOM
## Nút
Đối với HTML DOM, cấu trúc dạng cây gọi là DOM Tree có nghĩa là mọi thành phần đều được xem là 1 nút (node), được biểu diễn trên 1 cây . Các phần tử khác nhau sẽ được phân loại nút khác nhau nhưng quan trọng nhất là 3 loại: nút gốc (document node), nút phần tử (element node), nút văn bản (text node).
 - Nút gốc: thường được biểu diễn bởi thẻ <html> là thành phần của HTML.
 - Nút phần tử: biểu thị cho 1 phần tử HTML.
 - Nút văn bản: mỗi đoạn kí tự trong tài liệu HTML, bên trong 1 thẻ HTML đều là 1 nút văn bản. Đó có thể là tên trang web trong thẻ <title>, tên đề mục trong thẻ <h1>, hay một đoạn văn trong thẻ <p>.
Ngoài ra còn có nút thuộc tính (attribute node) và nút chú thích (comment node).
![node](https://viblo.asia/uploads/1a8059f6-d788-4368-b472-5983fb5ccbf5.png)
Quan hệ giữa các nút
- Nút gốc (root document) luôn luôn là nút đầu tiên.
- Tất cả các nút không phải là nút gốc và đều chỉ có 1 nút cha (parent).
- Một nút có thể có một hoặc nhiều con, hoặc cũng có thể không có con nào.
- Những nút anh em (siblings) thì có cùng nút cha.
- Trong các nút anh em (siblings), nút đầu tiên được gọi là anh cả (firstChild) và nút cuối cùng là em út (lastChild).
# Thuộc tính và phương thức thường gặp
Các khái niệm này khá là quen thuộc, các bạn có thể tìm trong W3Schools

# Truy xuất DOM
## Truy xuất gián tiếp
Mỗi nút trên cây DOM đều có 6 thuộc tính quan hệ để giúp bạn truy xuất gián tiếp theo vị trí của nút:
- Node.parentNode: tham chiếu đến nút cha của nút hiện tại, và nút cha này là duy nhất cho mỗi nút. Do đó, nếu bạn cần tìm nguồn gốc sâu xa của 1 nút, bạn phải nối thuộc tình nhiều lần, ví dụ Node.parentNode.parentNode.
- Node.childNodes: tham chiếu đến các nút con trực tiếp của nút hiện tại, và kết quả là 1 mảng các đối tượng. Lưu ý rằng, các nút con không bị phân biệt bởi loại nút, nên kết quả mảng trả về có thể bao gồm nhiều loại nút khác nhau.
- Node.firstChild: tham chiếu đến nút con đầu tiên của nút hiện tại, và tương đương với việc gọi Node.childNodes[0].
- Node.lastChild: tham chiếu đến nút con cuối cùng của nút hiện tại, và tương đương với việc gọi Node.childNodes[Element.childNodes.length-1].
- Node.nextSibling: tham chiếu đến nút anh em nằm liền kề sau với nút hiện tại.
- Node.previousSibling: tham chiếu đến nút anh em nằm liền kề trước với nút hiện tại.
![node](https://viblo.asia/uploads/2e24abf1-7388-4af6-b7a4-214476cb939f.png)

## Truy xuất trưc tiếp
Truy xuất trực tiếp sẽ nhanh hơn, và đơn giản hơn khi bạn không cần phải biết nhiều về quan hệ và vị trí của nút. Có 3 phương thức để bạn truy xuất trực tiếp được hỗ trợ ở mọi trình duyệt:
- document.getElementById('id_cần_tìm')
- document.getElementsByTagName('div')
- document.getElementsByName('tên_cần_tìm')
![node](https://viblo.asia/uploads/23a057e4-0dd5-45ea-8f43-217c72534347.png)
Các trình duyệt hiện đại sau này (Chrome) có hỗ trợ thêm các phương thức truy xuất mạnh mẽ và linh hoạt hơn nhiều, thậm chí hỗ trợ truy xuất theo vùng chọn CSS phức tạp như vùng chọn jQuery (một thư viện Javascript mạnh và đáng dùng để tối ưu hóa hiệu quả công việc cũng như tiết kiệm thời gian).
- document.querySelector('#id p.class'): truy xuất đến vùng chọn và trả về kết quả tham chiếu đầu tiên.
- document.querySelectorAll('#id p[class^=test]'): tương tự querySelector nhưng trả về mảng các tham chiếu.
- document.getElementsByName('class1 class2'): tham chiếu đến tất cả các nút có thuộc tính className chứa tất cả các tên lớp cần tìm.
# Kết luận
Chung quy lại chúng ta đã cùng nhau tìm hiểu các khái niệm cơ bản về DOM và cách thao tác với nó. Đó chỉ là những kiến thức hết sức cơ bản, tuy nhiên bạn cũng có thể thấy DOM quan trọng như thế nào.