---
layout: post
title:  "So sánh điểm khác nhau giữa Angular 1 và Angular 2"
date:   2017-05-20
categories: jekyll update
---
![So sánh điểm khác nhau giữa Angular 1 và Angular 2]
-Được phát triển mới hoàn toàn, ko phải là bản nâng cấp cho angularjs 1.x

-Hiện tại đang là phiên bản beta: 2.0.0-beta.9/ 10-Mar-2016 00:29

-Angularjs 1.x ko hỗ trợ mobile, angularjs 2.x thì có.

-Angularjs 1.x hỗ trợ ES5, ES6, Dart, Angularjs 2.x có thêm Typescript.

-Angularjs 1.x dễ cài đặt chỉ cần chèn thư viện và chạy. ở Angularjs 2.x có nhiều thư viện cần phải thiết lập phức tạp hơn để chạy đc.

-Angular CLI hỗ trợ tạo ứng dụng Angularjs 2.x

-Angular 2.x không còn controllers and $scope

-Controller:

Angular 1.x Controller
```bash
var myApp = angular
   .module("myModule", [])
   .controller("productController", function($scope) {
     var prods = { name: "Prod1", quantity: 1 };
     $scope.products = prods;
});
```
Angular 2 Components using TypeScript
```bash
import{ Component } from 'angular2/core';
@Component({
  selector: 'prodsdata',
  template: `
    <h3>{{prods.name}}</h3> `
})
exportclassProductComponent {
  prods = {  name: 'Prod1', quantity: 1 };
}
```
-Angular 1.x có 2 cách sủ dụng bootstrap. Một là sửa dụng thuộc tính ng-app, hai là sử dụng code.
```bash
angular.element(document).ready(function() {
      angular.bootstrap(document, ['myApp']);
});
```
Trong Angular 2, không còn sửa dụng ng-app. Chỉ có thể sử dụng bootstrap qua code.
```bash
import { bootstrap } from 'angular2/platform/browser';
import { ProductComponent } from './product.component';
bootstrap(ProductComponent);
```
–Structural directives: cú pháp thay đổi đó là ng-repeat được thay thế bằng *ngFor.
Angular 1.x structural directives
```bash
<ul>
   <li ng-repeat="technology in technologies">
     {{technology.name}}
   </li>
</ul>
<div ng-if="technologies.length">
   <h3>You have {{technologies.length}} technologies.</h3>
</div>
```
Angular 2.x structural directives
```bash
<ul>
  <li *ngFor="let technology of technologies">
    {{technology.name}}
  </li>
</ul>
<div *ngIf="technologies.length">
  <h3>You have {{technologies.length}} technologies.</h3>
</div>
```
-Angular 2, các biến local đều có sửa dụng tiền tố là ký tự (#) trong vòng lặp *ngFor.

-Angular 2 sử dụng cú pháp camelCase. Ví dụ, ng-class bây giờ là ngClass và ng-model bây giờ là ngModel.

-Thay đổi về HTML DOM. Angular 1.x là  ng-href, ng-src, ng-show, ng-hide ... Angular 2.x sẽ là href, src, hidden. Các sự viện cũng thay đổi ng-click and ng-blur => (click), (blur).

Angular 1.x
```bash 
<button ng-click="doSomething()">
```
Angular 2.x
```bash 
<button (click)="doSomething()">
```
– Angular 1.x, ng-bind, Angular 2.x được thay thế bằng thuộc tính trong ngoặc vuông [property]

Angular 1.x, data binding
```bash
<input ng-bind="technology.name"></input>
```
Angular 2,  data binding
```bash
<input [value]="technology.name"></input>
<div [style.color]="color">Some text...</div>
```
-Angular 1.x sử dụng ng-model , nhưng Angular 2 được thay thế bằng [(ngModel)].
Angular 1.x
```bash
<input ng-model="technology.name"></input>
```
Angular 2x
```
<input [(ngModel)]="technology.name"></input>
```
-Angular 1.x có thể định nghĩa service theo 5 cách: Factory, Service, Provider, Constant, Values. Còn Angular 2.x có một class duy nhất định nghĩa service.

–Dependency Injection trong Angularjs 2.x được thông qua bởi constructor.

Angular 1.x, sử dụng $routeProvider.when() để cấu hình route. Angular 2, dùng @RouteConfig{(...}). ng-view present in Angular 1.x is replaced with <router-outlet>.
-Angular 2.x  có performance tốt hơn Angular 1.x

-Cài đặt project angular 2.x bằng angular CLI:
```bash
npm install angular-cli -g
npm cache clean
ng new application-name
ng new AngularCLIDemoApp
ng serve
ng build
ng test
ng generate class Demo
ng g component demo-component
ng g service demo-service
ng g directive demo-directive
ng g pipe demo-pipe
ng g route test
ng g route demo
ng g route demo --inline-template --inline-style
ng lint
ng format
```
