---
layout: post
title:  "20 câu hỏi tuyển dụng cơ bản cho Junior iOs Developer ( Phần 2)"
date:   2016-12-30 14:02:57 +0700
categories: jekyll update
---
Tiếp theo phần 1, hôm nay chúng ta sẽ cùng nhau đi tiếp 10 câu hỏi còn lại trong bộ sưu tập những câu hỏi tuyển dụng cơ bản dành cho lập trình viên iOS mới ra trường và còn ít kinh nghiệm:

<strong>11 ) Phân biệt giữa frame và bounds  </strong>

Frame là tọa độ và kích thước của view trong View cha của nó

Bounds là tọa độ và kích thước của view trong hệ tọa độ của chính nó

<strong>12 )  Mục đích sử dụng của khái niệm "reuseIdentifier" ?</strong>

 Dùng để tái sử dụng một đối tượng đã được khởi tạo và cấp phát trước đó. Ví dụ: tableView.dequeuereuseablecellwithIdentifier

<strong>13 ) Có bao nhiêu Cell được khởi tạo khi lần đầu tiên bạn load một UITableView? Mỗi khi scroll màn hình thì có bao nhiêu Cell được thêm vào?</strong>

UITableView sẽ khởi tạo một số lượng object Cell vừa đủ để hiển thị trên màn hình của thiết bị. Và với cơ chế reuseIdentifier, UITableView sẽ không tạo thêm Cell nào cả, mà sẽ tái sử dụng các đối tượng sẵn có để tránh tình trạng giật, lag máy.

<strong>14 )  Định nghĩa atomic và nonatomic.</strong>

Khi bạn có nhiều hơn một thread (luồng), nó có thể cho các setter và getter được gọi cùng một lúc. Điều này có nghĩa rằng getter / setter có thể bị gián đoạn bởi hoạt động khác, có thể dẫn đến dữ liệu bị hỏng.

Thuộc tính Atomic sẽ ngăn chặn điều này xảy ra, đảm bảo rằng các hoạt động được, hoặc thiết lập đang làm việc với một giá trị đầy đủ. Tuy nhiên, điều quan trọng là phải hiểu rằng đây chỉ là một khía cạnh của thread-safe-using (luồng an thoàn) và thuộc tính Atomic không đảm bảo là mã của bạn là thread-safe.

<strong>15)  Sự khác nhau giữa weak và strong ?</strong>

Strong pointer là một con trỏ, trỏ đến một đối tượng và sở hữu (own) đối tượng đó. 
Weak pointer là một con trỏ, trỏ đến một đối tượng nhưng không sở hữu (own) đối tượng đó.
 
 Khi tạo ra một reference strong đến một đối tượng, retainCount của đối tượng đó tăng lên 1
- Khi release một tham chiếu strong đến một đối tượng, retainCount của đối tượng giảm đi 1
Nghĩa là: tham chiếu strong sở hữu đối tượng mà nó tham chiếu đến, nó quyết định đến sự tồn tại của đối tượng.
- Khi retainCount của đối tượng về 0 thì đối tượng được giải phóng hoàn toàn khỏi bộ nhớ
- Khi retainCount > 0 và gán object = nil thì đối tượng cũng được giải phóng hoàn toàn khỏi bộ nhớ
- Khi tạo ra một tham chiếu "weak" đến một đối tượng, retainCount của đối tượng đó không tăng lên 1.
- Khi release một tham chiếu "weak" đến một đối tượng, retainCount của đối tượng đó không bị giảm đi 1.
weak được sử dụng chủ yếu trong kết nối IBOutlet và sử dụng để tránh trường hợp retain cycle
<br/>
<strong>16)  Liệt kê 5 trạng thái của 1 ứng dụng iOs</strong>

Not running: Ứng dụng chưa được mở hoặc đang mở thì bị đóng bởi hệ thống.
Inactive: Ứng dụng đang chạy trên màn hình nhưng không nhận được thao tác nào của người dùng. Trạng thái này diễn ra khi ứng dụng đang trong giai đoạn chuyển từ màn hình này sang màn hình khác.
Active: Đây là chế độ bình thường của ứng dụng, ứng dụng chạy trên mà hình và nhận được đầy đủ các thao tác của người dùng.
Background: Ở trạng thái này, ứng dụng sẽ chạy ở dưới background, khi app đang chạy bình thường mà chúng ta ấn nút Home thì ứng dụng sẽ chuyển vào trạng thái này.
Suspended: Ứng dụng nằm ở dưới background nhưng code sẽ không chạy.
<br/>
<strong>17)  Category là gì và cách sử dụng ?</strong>

Để thêm một phương thức vào trong class mà không muốn mở rộng nó, chúng ta sử dụng Category. Categories là cách để chúng ta phân chia một lớp khởi tạo ra nhiều file khác nhau. Mục đích của việc này là để giảm bớt gánh nặng của việc duy trì các đoạn cơ sở code  lớn bằng việc modul hóa thành một lớp. Điều này giúp bạn tránh khỏi việc viết các tập tin với hơn 10000 dòng code mà không thể ứng dụng, di chuyển đến chỗ khác.

<strong>18) Phân biệt viewDidLoad và viewDidAppear? Nên dùng phương thức nào để load dữ liệu từ server để hiển thị trên view? </strong>

– viewDidLoad: Được gọi một lần khi lần đầu tiên đối tượng view của đối tượng UIViewController hiển thị.

– viewDidAppear: Tương tự viewWillAppear, có thể được gọi nhiều lần. Method này được gọi sau khi view đã hiển thị.

Nếu data là dạng tĩnh và không thay đổi nhiều, chúng ta nên load chúng ở phương thức viewDidlLoad. Tuy nhiên, nếu data của bạn là động và thay đổi thường xuyên, chúng ta nên load tại phương thức ViewDidAppear. Và lưu ý là tại 2 phương thức, data cần phải load theo dạng bất đồng bộ ở một thread khác để tránh ảnh hưởng đến UI.

<strong>19) Điều gì xảy ra khi bạn gọi một method trên một con trỏ nil? </strong> 

Một message được gửi tới một nil object là chấp nhận được trong Objective-C. Đây là một tính năng của ngôn ngữ này và không được xét là lỗi.

<strong>20)  Duyệt phần tử ở đâu thì nhanh hơn? NSSet hay NSArray?</strong>

Khi thứ tự của các phần tử không quan trọng thì quá trình duyệt phần tử tại NSSet sẽ nhanh hơn vì NSSet sử dụng hash value để tìm phần tử, giống như từ điển. Còn NSArray sẽ duyệt qua nội dung của từng phần tử một.

