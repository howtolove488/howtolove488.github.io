---
layout: post
title:  "Giới thiệu về Angular 2"
date:   2017-04-27 14:02:57 +0700
categories: jekyll update
---
> Giới thiệu về Angular 2

# I. Angular 2 là gì ?
Angular 2 là 1 framework UI để xây dựng ứng dụng web trên desktop và mobile.
Nó được xây dựng dựa trên Javascript. Chúng ta có thể dùng nó để xây dựng 1 ứng dụng client side thú vị dùng HTML, CSS và Javascript.
Angular 2 có rất nhiều cải tiến so với Angular 1 để dễ dàng học và phát triển các ứng dụng quy mô doanh nghiệp.
Với angular 2 thì chúng ta dễ dàng xây dựng được 1 ứng dụng có thể dễ dàng mở rộng, bảo trì, kiểm nghiệm và chuẩn hóa ứng dụng của mình.
# II. Các tính năng trong angular 2
Dưới đây là các tính năng nổi bật trong angular 2

Two-way data binding
Đây là 1 trong những tính năng tuyệt với nhất trong angular 2. Dữ liệu được binding một cách tự động và nhanh chóng, những thay đổi trong view sẽ được tự động cập nhật vào trong các component class.
Powerful routing support
Angular 2 hỗ trợ mạnh mẽ các routing thông qua cách tải trang không đồng bộ trên cùng 1 trang cho phép chúng ta tạo ra 1 single page application.
Expressive HTML
Angular 2 cho phép chúng ta dùng các cấu trúc lập trình như câu lệnh if, vòng lặp for, .. để render và kiểm soát các trang HTML.
Modular by design
Angular 2 được thiết kế theo hướng modul hóa để tổ chức và quản lý code 1 cách tốt hơn.
Built in back end support
Angular 2 được xây dựng để hỗ trợ việc giao tiếp với back-end servers và thực thi bất kỳ business logic hoặc lấy dữ liệu.
Active community
Angular 2 được hỗ trợ bởi google và có 1 cộng đồng đông đảo sẵn sàng hỗ trợ và giải đáp bất cứ câu hỏi nào của bạn.
# III. Sự khác biệt chính giữa angular 1 và angular 2
## 1. Hỗ trợ ES6
Angular 2 hoàn toàn được viết bằng Typescript. Điều đó đồng nghĩa là nó hỗ trợ cho ES6 Modules, class frameworks, ..

## 2. Components là 1 controller mới
Trong angular 1 chúng ta có controllers còn trong angular 2 thì controller được thay thế bởi components.
Controller và view trong angular 1 được định nghĩa như sau.
```bash
//View
<body ng-controller=’appController’>
    <h1>vm.message<h1>
</body>
//Controller
angular.module(‘app’).controller(‘appController’,appcontroller) {
    message=’Hello Angular2’;
}
```
Còn trong angular 2 thì chúng ta sử dụng component.
```bash
import { Component } from '@angular/core';
 
@Component({
    selector: 'app',
    template: '<h1>{{message}} </h1>'
})
export class AppComponent  { 
    message: string=’Hello Angular2’;
}
```
Trong angular 2, 1 component đại diện cho 1 phần tử UI. Chúng ta có thể có nhiều component trong 1 single web page. Các component là độc lập với nhau và quản lý 1 vùng của trang. Component có thể có component con và component cha.

## 3. Directives
Angular 1 có rất nhiều directives. Và một vài directives được sử dụng nhiều nhất là ng-repeat và ng-if.
```bash
<ul>
    <li ng-repeat =customer in vm.customers>
       {{customer.name}}
    </li>
</ul>
 
<div ng-if=”vm.isVIP”>
    <h3> VIP Customer </h3>
</div>
```
Trong angular 2 cũng có directives nhưng với 1 cú pháp khác. Nó có 1 dấu * trước tên của directives.
```bash
<ul>
    <li *ngFor =#customer of customers>
        {{customer.name}}
    </li>
</ul>
 
<div *ngIf=”vm.isVIP”>
    <h3> VIP Customer </h3>
</div>
```
Trong angular 2 ng-style, ng-src , ng-href đã biến mất và chúng được thay thế bởi property binding.
Việc tạo 1 custom directives là cực kỳ đơn giản trong angular 2
```bash
@Directive({
    selector: '[MyDirective]'
})
class MyDirective {
}
```
## 4. Data Bindings
### 4.1 Interpolation
//Angular 1
```bash
<h3> {{vm.customer.Name}}</h3> 
``` 
//Angular 2
```bash
<h3> {{customer.Name}}</h3>
```
### 4.2 One way Binding
//Angular 1
```bash
<h3> ng-bind=vm.customer.name></h3>
```
//Angular 2
```bash
<h3 [innerText]=”customer.name” ></h3>
```
Trong angular 2, chúng ta có thể bind đến bất cứ thuộc tính nào của phần tử html

### 4.3 Event Binding
//Angular 1
```bash
<button ng-click=”vm.save()”>Save<button> 
``` 
//Angular 2
```bash
<button (click)=”save()”>Save<button>
```
Trong angular 1 dùng directive ngClick để bind 1 sự kiện còn trong angular 2 directive ngClick đã được loại bỏ và chúng ta có thể bind trực tiếp đến DOM events.

### 4.4 Two- way binding
//Angular 1
```bash
<input ng-model=”vm.customer.name”> 
``` 
//Angular 2
```bash
<input [(ng-model)]=”customer.name”>
```
## 5. Filters được đổi tên thành Pipes
Trong angular 1, chúng ta dùng Filters như sau
```bash
<td>{{vn.customer.name | uppercase}}</td>
```
Còn trong angular 2 chúng ta cũng dùng 1 cú pháp tương tự nhưng tên chúng là pipes

```bash
<td>{{customer.name | uppercase}}</td>
```
## 6. Platform specific Bootstrap
Trong angular 1 thì chúng ta dùng directive ng-app trong HTML
```bash
<body ng-app=’app’>
```
Còn trong angular 2 thì nó sẽ phức tạp hơn 1 chút :D
```bash
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import {AppModule } from './app.module';
 
platformBrowserDynamic().bootstrapModule(AppModule);
```
## 7. Services
Trong angular 1 có Services, Factories , Providers, Constants và values và chúng ta injected vào trong controller để có thể dùng, còn trong angular 2 tất cả mọi thứ trên đều được gộp vào Service.Class

Hy vọng qua bài viết này sẽ giúp các bạn có 1 cái nhìn tổng quan về Angular 2. Thankyou