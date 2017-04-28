---
layout: post
title:  "Nổi trội của angular js 2 so với angular version 1.x"
date:   2017-04-30 14:02:57 +0700
categories: jekyll update
---
> Nổi trội của angular js 2 so với angular version 1.x

Chào các bạn!

Như tiêu đề, hôm nay mình xin giới thiệu đôi nét cơ bản về angular js 2
Như các bạn đã biết, @Angular2@ vừa được release cách đây không lâu sau một thời gian phát hành bản beta. Vậy angular 2 có những đặc điểm gì mà chúng ta cần phải lưu tâm, có những đặc điểm gì vượt trội so với angular version 1.x. Chúng ta cùng tìm hiểu nhé :). Let's go!!!!

Thứ nhất, angular 2 có thể build trên bất kì nền tảng nào, cross-flatform
Tiếp theo Angular js 2 còn khắc phục những thiếu sót, hạn chế của version trước đó, như cải thiện về performance, tối ưu hóa cho SEO, có tính Productivity cao
Angular 2 đã lược bỏ controller, $scope thay bằng các Component
Các Service có thể build được bằng cách tạo các class dựa trên nền tảng ES6 so với kiểu function của Angular 1
Không những vậy, Angular 2 còn được xây dựng trên nền tảng Typescript - hỗ trợ OOP rất tốt cho Javascript
Và còn nhiều nét ưu việt nữa. Mình chỉ tóm gọn các đặc điểm vượt trội như thế thôi. Còn những nét khác các bạn có thể google thêm nhé :D
Sau khi biết những đặc điểm nổi trội của Angular version 2 như vậy rồi, thì chúng ta đi tiến hành tìm các thành phần chính của Angular 2 nhé!

Cấu trúc Angular 2
Một project trong Angular 2 cần có các thành phần như sau:

Module
Components
Service
Object
Pipe
Nào chúng ta cùng đến với yếu tố đầu tiên là Module nhé :)

# I-Module

Nhiệm vụ của module là làm gì?
Load các package cần thiết từ core-libraries
Load các Component
Load các Service
Load các Provider
Và cuối cùng là khởi tạo ứng dụng của chúng ta
Ví dụ:
```bash
import {HeroService} from "./services/hero.service";
import {DashboardComponent} from "./components/dashboard.component";
import {HeroSearchComponent} from "./components/hero-search.component";

@NgModule({
  imports: [
      BrowserModule,
      FormsModule,
      HttpModule,
      InMemoryWebApiModule.forRoot(InMemoryDataService),
      routing
  ],
  declarations: [
      AppComponent,
      DashboardComponent,
      HeroDetailComponent,
      HeroesComponent,
      HeroSearchComponent
  ],
  providers: [
      HeroService
  ],
    bootstrap: [AppComponent]
})

export class AppModule { }
```
Qua đoạn code trên, ta thấy:

Để App chạy Ok thì cần các thư viện của angular như: BrowserModule, FormsModule, HttpModule, và một vài thư viện khác.
Đồng thời chúng ta còn thấy sự xuất hiện của các Component trong file này, như là: AppComponent, DashboardComponent, HeroDetailComponent, ...
Các service như: HeroService
Qua đây các bạn đã hình dung được phần nào về vai trò, nhiệm vụ của Module rồi đúng ko nào :)

Tiếp theo thì chúng ta cũng tìm hiểu về component trong angular 2 nhé

# II-Component

Nhiệm vụ của module là làm gì?
Định nghĩa các selector mà chúng ta sẽ sử dụng chúng trong view
Định nghĩa template mà các bạn muốn sử dụng(theo kiểu partial view đó các bạn)
Định nghĩa các method mà bạn tương tác trên view, ví dụ: click vào button A sẽ làm gì, button B sẽ làm gì, đại loại như vậy
Gọi các service cần thiết để cung cấp các method, các thuộc tính cho component đang sử dụng
Nói thì loằng ngoằng quá, nhiều khi diễn tả không hết ý :v. Thôi thì các bạn xem ví dụ sau đây để hiểu thêm nha:

Ví dụ
```bash
@Component({
    selector: 'channel-create',
    providers: [ChannelService],
    templateUrl: '/partials/channels/channel-create'
})

export class ChannelCreateComponent implements OnInit{
    channelForm: any;
    channel = new Channel;
    submit: boolean = false;
    companies: Company[];
    errorMessage: string;
    categories: ChannelCategory[];
    auth_type = [
        'app', 'oauth', 'udid', 'push_notification', 'none'
    ];
    public = [false, true];

    constructor (
        private channelService: ChannelService,
        private router: Router,
        private formBuilder: FormBuilder
    ) {
        this.channelForm = this.formBuilder.group({
            'channel_category_id': ['', Validators.required],
            'company_id': ['', Validators.required],
            'identifier': ['', Validators.required],
            'name': ['', Validators.required],
            'description': ['']
        });
    }

    ngOnInit() {
        this.channelService.getCategories()
            .subscribe(
                categories => this.categories = categories,
                error => this.errorMessage = <any> error
            );
        this.channelService.getCompanies()
            .subscribe(
                companies => this.companies = companies,
                error => this.errorMessage = <any> error
            );
    }

    save() {
        this.channelService.create(this.channel)
            .subscribe(
                channel => this.channel = channel
            );
        this.router.navigate(['/channels']);
    }
}
```
Ở ví dụ này, chúng ta thấy các đặc điểm như:

Định nghĩa selector mà chúng ta muốn dùng ở view
Dùng template bằng thuộc tính templateUrl
Gọi service để sử dụng các phương thức đã định nghĩa trong service, như method create()
Định nghĩa method save để lưu lại các thao tác của người dùng trên view
# III-Service

Nhiệm vụ
Tương tác với back-end(server) để CRUD data từ phía back-end thông qua Observable
Ví dụ:
```bash
@Injectable()
export class ChannelService {
    private baseUrl = 'http:/localhost/';
    private headers = new Headers({'Content-Type': 'application/json'});

    constructor(private http: Http) { }

    getChannels(): Observable<Channel[]> {
        return this.http.get(this.baseUrl + 'channels')
            .map(this.extractData)
            .catch(this.handleError);
    }

    create(channel: Channel): Observable<Channel> {
        return this.http.post(this.baseUrl + 'channels', JSON.stringify(channel), {headers: this.headers})
            .map(this.extractData)
            .catch(this.handleError);
    }

    delete(id: number): Observable<any> {
        return this.http.delete(this.baseUrl + 'channels/' + id)
            .map((res: Response) => {
                return res.status;
            })
            .catch(this.handleError);
    }

    update(channel: Channel): Observable<Channel> {
        return this.http.put(this.baseUrl + 'channels/' + channel.id, JSON.stringify(channel), {headers: this.headers})
            .map(this.extractData)
            .catch(this.handleError);
    }

    private extractData(res: Response) {
        return res.json();
    }

    private handleError(error: any) {
        let errMsg = (error.message) ? error.message : error.status ? `${error.status} - ${error.statusText}` : 'Server error';
        console.log(errMsg);

        return Observable.throw(errMsg);
    }
}
```
Qua ví dụ trên ta thấy là:
Service sẽ tương tác với server để CRUD data thông qua các phương thức như getChannels(), create(), ... thông qua thư viện hỗ trợ là Observable

# IV-Object

Nhiệm vụ
Định nghĩa các thuộc tính cho đối tượng để sử dụng khi CRUD với server
Ví dụ:
```bash
export class Channel {
    id: number;
    name: string;
    description: string;
    device_type: string;
    auth_type: string;
    auth_driver: string;
    auth_driver_arguments: string;
    status: number;
    public: boolean;
    sort: number;
    created_at: string;
}
```
Như các bạn thấy ở ví dụ trên: đối tượng channel của chúng ta có các thuộc tính như: id, name, description, .... Dựa vào những thuộc tính này, chúng ta có thể thao tác dữ liệu trên view, hoặc khi update thì chúng ta cũng phải đưa đúng kiểu và đầy đủ các thuộc tính mà đã định nghĩa trong Object này

# V-Pipe
Chúng ta đến với phần cuối cùng là Pipe

Vai trò của Pipe
Nhiệm vụ của Pipe là filter data trên view
ví dụ:
```bash
import { Pipe, PipeTransform } from '@angular/core';
import { Channel } from "./channel";

@Pipe({
    name: 'ChannelFilterPipe',
})

export class ChannelFilterPipe implements PipeTransform{
    transform(channels: Channel[], input: any) {
        if (input !== '') {
            return channels.filter(channel =>
                channel.identifier.startsWith(input) ||
                channel.identifier.includes(input) ||
                channel.identifier === input
            );
        }

        return channels;
    }
}
```
Đoạn code trên nhằm minh họa cho chúng ta khả năng tìm kiếm trên view hiện tại của bạn mà không cần phải reload trang

Qua bài viết trên, phần nào chúng ta cũng hiểu được cấu trúc của một ứng dụng trong angular 2 rồi phải không nào.

Ở các bài viết tiếp theo thì mình sẽ giới thiệu sâu hơn từng phần cho các bạn hiểu thêm
Xin chào và hẹn gặp lại ở bài viết tiếp theo nhé!!