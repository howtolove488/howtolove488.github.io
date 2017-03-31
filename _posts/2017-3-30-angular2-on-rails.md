![How to have Angular2 and Rails coexist peacefully on your laptop and servers](https://i0.wp.com/sungthecoder.com/wp-content/uploads/2017/01/trees-railroads-railroad-e1485229908918.jpg?fit=1024%2C617)

> How to have Angular2 and Rails coexist peacefully on your laptop and servers

Most of the front-end tutorials assume that your JS application runs on a separate server to the backend application and the front-end code has different codebase. It makes sense when you have independent teams to develop the front-end and the backend. But if you work in a small team of full stack developers, having separate codebase becomes an overhead.

One option of integrating the Angular app with Rails would be putting all javascript code within Rails directory structure, i.e., ‘app/assets/javascript.’

However, this option only makes sense when you have a little JavaScript code, and you want Rails asset pipeline to concatenate all your javascript codes. So most of the case, this would not be the solution because:

1. TypeScript is the language choice of Anguar2. And Rails’ asset pipeline does not know how to handle typescript. You don’t have to write Angular2 app in TypeScript, but you don’t want to give up the option just because the environment you chose.
2. And the Angualar2 app may want to be treated as the first class app. Angular2 app will play vital importance in your whole application. Placing that critical code somewhere deep inside the closet, ‘app/assets/javascript/ng2-app/…’ doesn’t seem to be very nice.

In this article, I would like to show how to configure Rails and Angular2, so that Rails app and the Angular2 app can coexist within one repository.

Followings are the result I would like to achieve at the end of this article:

* Your Angular2 app’s source code live in an independent directory structure, e.g. ‘RAILS_ROOT/ng2-app’
* Your Angular2 Service can use relative path for API calls because they will live on the same server. No need for CORS configuration.
* During development, you can use rails development server and angular development server. Rails server provides all the API calls, and Angular2 server helps you with the Angular2 app.
* At deployment time, asset precompile will include your Angular 2 bundles so that Rails controller can serve the Angular2 app.

#1. Create a Rails app.

The first thing you would need is a working Rails app. If you already have an existing rails app, that’s great. But if you’d like to start from scratch, here’s how we start.

Let’s create a Rails app and call it ‘rails_ng2.’

```bash
$ rails new rails_ng2
$ cd rails_ng2
```

And let’s add a resource and call it ‘widget.’ The widget has three properties: name, description, and price.

```bash
$ rails g scaffold Widget name description price:float
```

![1](https://i0.wp.com/sungthecoder.com/wp-content/uploads/2017/01/rails_g_scafold.png?resize=1024%2C864)

Let’s migrate the database.

```bash
$ rake db:migrate
```
![2](https://i0.wp.com/sungthecoder.com/wp-content/uploads/2017/01/rake_db_migrate.png?resize=1024%2C109)

Run the rails development server
```bash
$ rails s
```

Then you already have a working Rails app! Point your browser to http://localhost:3000/widgets; then you should be able to view and create some widgets.

![3](https://i0.wp.com/sungthecoder.com/wp-content/uploads/2017/01/localhost4200Widgets.png?w=804)  

#2. Add API endpoint

Now it’s time to build API endpoints for these widgets you’ve just created. It seems to be redundant to write another controller for the same behavior, but they serve two different audiences(human vs. machine) and purposes(rendering HTML and providing data). So it is always a good idea to create a separate endpoint for APIs.

With Rails, it is always easy to create another controller:
```bash
$ rails g controller api/widgets
```
And edit widgets controller file.

```bash
class Api::WidgetsController < ApplicationController
  before_action :set_widget, only: [:show, :update, :destroy]
  respond_to :json

  def index
    @widgets = Widget.all
    render json: @widgets
  end

  def show
    render json: @widget
  end

  def create
    @widget = Widget.new(widget_params)
    if @widget.save
      render json: @widget
    else
      render json: @widget.errors, status: :unprocessable_entity
    end
  end

  def update
    if @widget.update(widget_params)
      render json: @widget
    else
      render json: @widget.errors, status: :unprocessable_entity
    end
  end

  def destroy
    @widget.destroy
    head :no_content
  end

  private
  # Use callbacks to share common setup or constraints between actions.
  def set_widget
    @widget = Widget.find(params[:id])
  end

  # Never trust parameters from the scary internet, only allow the white list through.
  def widget_params
    params.require(:widget).permit(:name, :description, :price)
  end
end
```   

Add the controller to the route configuration.    
```bash
namespace :api do
  resources :widgets
end
```  

You can point your browser to http://localhost:3000/api/widgets and see JSON representation for your widget resources.

#3. Add Angular2 App
Now you have a working Rails app and API endpoint; it’s time to build an Angular2 app to consume the API.

With Angular CLI, it becomes easy to create an angular2 app. If you don’t have Angular CLI, install it with npm.
```bash
$ npm install -g angular-cli
``` 
And create an Angular2 app:
```bash
$ ng new ng2-app
 ``` 
This command will create an Angular2 app under ‘ng-app’ directory.
```bash
$ cd ng2-app
ng2-app$ npm start
``` 
Point your browser to http://localhost:4200 and you will see this.    
![4](https://i0.wp.com/sungthecoder.com/wp-content/uploads/2017/01/localhost4200.png?w=970)

#4. Add components and routing to the Angular2 app

Let’s create two components; home and widgets. With Angular CLI, it is one line task:
```bash
ng2-app$ ng g component widgets
 create src/app/widgets/widgets.component.css
 create src/app/widgets/widgets.component.html
 create src/app/widgets/widgets.component.spec.ts
 create src/app/widgets/widgets.component.ts
ng2-app$ ng g component home
 create src/app/home/home.component.css
 create src/app/home/home.component.html
 create src/app/home/home.component.spec.ts
 create src/app/home/home.component.ts
``` 
Let’s add a router to load one of these components based on your URL.
```bash
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';
import { WidgetsComponent } from './widgets/widgets.component';
import { HomeComponent } from './home/home.component';
 
const routes: Routes = [
  {path: '', component: HomeComponent},
  {path: 'widgets', component: WidgetsComponent },
  {path: '*', redirectTo: ''}
];
 
@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule],
  providers: []
})
export class Ng2RestAppRoutingModule { }     
```
This routing configuration will have our app to load Widgets Component when we point the browser URL to ‘/widgets’ or Home Component otherwise.

And we will have to tell where to load these components. Add <router-outlet></router-outlet>  at the end of ‘ng-app/src/app/app.component.html‘

And for our convenience, let’s add a navigation menu to app component template.

-app/src/app/app.component.htmlXHTML
```bash
<h1> {{title}} </h1>

<nav class="mdl-navigation">
  <a [routerLink]="links.home">Home</a>
  <a [routerLink]="links.widgets>Widgets</a>
</nav>

<router-outlet></router-outlet>     
```
And let’s add it to the component class.
```bash
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'app works!';
  links = {
    home: ['/'],
    widgets: ['/widgets']
  };
}     
```

Now we will have to wire all these modules in app.module.ts

```bash
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';
import { Ng2RestAppRoutingModule } from './app-routing.module';

import { AppComponent } from './app.component';
import { HomeComponent } from './home/home.component';
import { WidgetsComponent } from './widgets/widgets.component';

@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    WidgetsComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    Ng2RestAppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }     
 ```
At this point, you should be able to run the Angular2 app on the separate development server, i.e., ng serve. You can click the navigation button and see it loads home and widgets components.

#5. Add a Service
Now we have built two separate apps: Rails app and Angular2 app, but they are not connected anyhow. In this step, we will create a service that can consume API that we’ve just set up on Rails app.

Angular CLI have service generator, but unlike the component generator, it creates files in the current directory. So we will have to create a directory and cd into the directory where we want the service modules to reside.
```bash
ng2-app/src/app$ mkdir shared
ng2-app/src/app$ cd shared
ng2-app/src/app/shared$ ng g service widgets
 create src/app/shared/widgets.service.spec.ts
 create src/app/shared/widgets.service.ts     
```

Let’s edit widgets service:
```bash
import { Injectable } from '@angular/core';
import { Http, Response } from '@angular/http';
import { Observable } from 'rxjs/Observable';
import 'rxjs/add/operator/map';

@Injectable()
  export class WidgetsService {
  private url = 'api/widgets';

  constructor (private http: Http) {}

  getWidgets() {
    return this.http.get(this.url)
      .map(res=> res.json())
  }
}      
```
Note that we didn’t add ‘http://localhost:3000’ suffix to the URL. We treat the API URL as if they are from the same origin.

Let’s use this service on our widgets component:
```bash
import { Component, OnInit } from '@angular/core';
import { WidgetsService } from '../shared/widgets.service'

@Component({
  selector: 'app-widgets',
  templateUrl: './widgets.component.html',
  styleUrls: ['./widgets.component.css']
})
export class WidgetsComponent implements OnInit {
  constructor (private widgetsService: WidgetsService) {}
  widgets = []

  ngOnInit() {
    this.widgetsService.getWidgets().
    subscribe(widgets => this.widgets = widgets)
  }
}     
```
And update the template to display widgets object:
```bash
<p>
  widgets works!
</p>

<pre>
  {{ widgets | json }}
</pre>     
```
And add the service module to the app modules.

```bash
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';
import { Ng2RestAppRoutingModule } from './app-routing.module';

import { AppComponent } from './app.component';
import { HomeComponent } from './home/home.component';
import { WidgetsComponent } from './widgets/widgets.component';

import { WidgetsService } from './shared/widgets.service';

@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    WidgetsComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    Ng2RestAppRoutingModule
  ],
  providers: [
    WidgetsService
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

If you run your Angular2 app and load Widgets Component, you will see 404 error on the developer console. That is because we do not have anything on http://localhost:4200/api/widgets. Our backend server is running on http://localhost:3000/api/widgets. So we have to tell our Angular server to look at http://localhost:3000 when we make an API call.

#6. Add dev server API proxy

In this step, we will tell Angular development server to look for localhost:3000 when we ask for something from /api/*.

First, you will have to create a proxy configuration file.

```bash
{
  "/api": {
    "target": "http://localhost:3000",
    "secure": false
  }
}
```
This configuration tells that when you see ‘/api’ prefix in your URL, forward the message to ‘http://localhost:3000’.

And let’s have our development server to use this proxy configuration. Open “package.json” file and replace start script with following:

```bash		
"start": "ng serve --proxy-config proxy.config.json"
```
Now, restart your Angular2 development server, you should see Widgets Component load all the widgets in JSON format!

Congratulations! You successfully wired Rails app and Angular2 app. You can do both frontend and backend development in one directory structure. Add more API endpoints; then you should be able to use them right away from your Angular2 app.

#7. Add controller to serve the Angular2 app

We’ve made a big progress, but we didn’t finish yet. Our ultimate goal is to have the Angular2 app available under our Rails app, not two separate servers: rails server running on port 3000, Angular server running on port 4200. Then we need to create a controller for our Angular2 app and router to call the controller.

```bash
$ rails g controller ng2_app
```

Let’s add the route to the configuration.
```bash
get "ng2-app",       controller: "ng2_app", action: "index"
get "ng2-app/*path", controller: "ng2_app", action: "index"
```

These two lines will let Rails to call index method on the Ng2AppController we’ve just created. We need to have these two lines so that Rails can ignore additional paths after ‘localhost:3000/ng2-app/, but Angular2 app will read path and decide its actions.

For example, if we enter ‘http://domain-name.com/ng2-app’, Rails will call index method of Ng2AppController. And then our Angular2 app’s routing system will decide to load Home Component.
But if we enter ‘http://domain-name.com/ng2-app/widgets’, Rails will ignore the last part: ‘widgets’ and still call index method of Ng2AppController. And the Anguar2 app’s routing will read the last part of the URL and load Widgets Component.

Since we don’t have any method defined in NgAppController, let’s add the method.
```bash
class Ng2AppController < ApplicationController
  def index
  end
end
```
And we need the view for this action.
```bash
<% content_for :head do %>
<base href="/ng2-app">
<% end %>

<app-root>Loading...</app-root>

<%= javascript_include_tag "ng2-app/inline" %>
<%= javascript_include_tag "ng2-app/styles.bundle" %>
<%= javascript_include_tag "ng2-app/main.bundle" %>
```
A few things are going on in this file:
1. <base href=”/ng2-app”> tells the Angular2 app to ignore first ‘/ng2-app’ from the path. For example, if the URL of the browser is pointing to ‘http://domain-name.com/ng2-app/‘, Anguar2 will only look at ‘/‘ and load Home Component. And if it sees ‘http://domain-nam.com/ng2-app/widgets‘, it will recognize ‘/widgets‘ and serve Widgets Component.
2. We must put this <base> tag within <head> tag, so we are wrapping <base> tag between <coneten_for :head>. We are going to add another yield  statement in the layout file later.
3. <app-root>Loading...</app-root>  is where the Angular2 app will live.
4. When you build the Angular2 app with ‘ng build,’ it will generate three files: ‘inline.js’, ‘styles.bundle.js’ and ‘main.bundle.js’ into ‘ng2-app/dist/’ directory. So we are loading these three files into our view template. Note that ‘ng2-app/’ prefix of the javascript filenames are not necessary. However, if you are planning to add more than one Anguar2 app under the same project, it is better to add the subdirectory.
Let’s open the application.html.erb and add one more line right before the closing tag of </head>
```bash
<%= yield :head %>
```
Now you open up the browser, and load ‘http://localhost:3000/ng2-app’ then you will see ‘Loading…’ but nothing happens. And the browser console will spit out 404 error for all the javascript files we try to load. That’s because Rails cannot find the javascript files we included in <%= javascript_include_tag %> .

#8. Let Rails know where to find Anuglar2 app
By default, Rails look for ‘app/assets/javascript’ and ‘lib/assets/javascript’, ‘vendor/assets/javascript’ directory to find the files we stated in <%= javascript_include_tag %> . But our Angular2 app does not reside on any of those directories. One solution would be copying Angular2 app files to one of standard search directories, but then we will have to copy files every time we build the Angular2 app and it’s too much work (I’m lazy). Instead, let’s teach Rails where to search.

Open ‘config/application.rb’ and add the following line between the ‘class Application < Rails::Application’ and ‘end’:
```bash
config.assets.paths << Rails.root.join("ng2-app", "dist")
```
This one line will tell Rails to look ‘RAILS_ROOT/ng2-app/dist/’ directory in addition to the Rails default asset paths for JavaScript files. So Rails will turn <%= javascript_include_tag "ng2-app/inline" %>  into <script src='ng2-app/dist/ng2-app/inline.js'>  for development environment.

But in the production environment, Rails will look for ‘/public/assets/ng-app/inline.[MD5-hash].js’. You will also have to tell Rails to include these file for precompilation. In order for that add the following line in “config/initializers/assets.rb”
```bash
Rails.application.config.assets.precompile +=
  %w( ng2-app/inline.js ng2-app/styles.bundle.js ng2-app/main.bundle.js)
  ```
The last step is to tell Angular2 to place bundle files into ‘dist/ng2-app/’ instead of ‘dist/.’ Open ‘ng2-app/angular-cli.json’. And change apps["outDir"]  value to dist/ng2-app.

```bash
{
  ...
  "apps": {
    ...
    "outDir": 'dist/ng2-app'
    ...
  }
}
```

Rebuild your Angular2 app with ng build  and restart Rails server with  rails s ; then you should be able to see your Angular2 app working on ‘http://localhost:3000/ng2-app’.

You may wonder why we bother to place our Angular2 app bundle files on ‘dist/ng2-app’ instead of the default ‘dist/’ directory. There are two reasons for that: self-documentation and future-proof.
Have left all the bundle files under ‘dist/’ directory, we could include the javascript files in our Rails view file like this: <%= javascript_include_tag 'main.bundle' %>  or <%= javascript_include_tag 'inline' %> . But then, two months later, you come back to this code and might have forgotten what the ‘min.bundle’ or ‘inline’ are. It would be easier to remember to if you write ‘ng2-app/main.bundle’ or ‘ng2-app/inline’.

Imagine six months later; your client wants to add another Single Page App under the same Rails app. That’s easy. You create ‘another-app’ directory on the RAILS_ROOT directory for another Angular2 app. And add config.assets.paths << Rails.root.join("another-app", "dist")  to your application.rb file. And your view file, you include the javascript bundles: <%= javascript_include_tag 'main.bundle' %> . Now you see the problem? Rails will include the JavaScript files from ‘ng-app/dist/’ directory, not from ‘another-app/dist/’ directory, because it will find main.bundle.js file from ‘ng-app/dist’ directory first.

Instead of a directory location, we could have changed bundled file name with a suffix, e.g., ng2-app.main.bundle.js, ng2-app.inline.js, etc. But at the moment, there is no option to change bundled file name in Angular CLI configuration, so I decided to change its build target directory: ‘ng2-app/main.bundle.js’ instead of ‘ng2-app.main.bundle.js’.


#9. Deploying to Heroku

This section is optional if you’d like to deploy your app to Heroku.

The small disadvantage of this approach is that you can’t have Heroku handle its asset precompilation process. Before precompilation assets, you will have to run ng build on your Angular2 app. That means you will have to push precompiled assets to Heroku.

At a minimum, you will need pg  and rails_12factor  gem for Heroku deployment. Add these two gems in your Gemfile.
```bash
group :production do
  # Heroku recommended gem
  gem 'pg'
  gem 'rails_12factor'
end
```
And install gems: bundle install

Initialize your GIT repository. And commit the files.
```bash
$ git init
$ git add .
$ git commit -m "initial commit"
   ```
Now, create a Heroku server to deploy your app:  heroku create

I don’t like to litter my master branch with precompiled assets in public directory. So I prefer to create a dist branch for deployment.
```bash
$ git co -b dist # create and switch to the dist branch
(dist)$ rake assets:clobber # remove old assets
(dist)$ (cd ng2-app; ng build) # build Angular2 app
(dist)$ RAILS_ENV=production rake assets:precompile # precompile assets
(dist)$ git add --all
(dist)$ git commit -m "initial deployment" # commit the change
(dist)$ git push heroku dist:master # push dist branch to heroku's master
	```
These are a lot of steps to remember. You will have to:

1. create and switch to a dist branch
2. remove all old assets
3. build the Anuglar2 app
4. precompile assets
5. commit the changes
6. and push the changes to Heroku.

I usually let CI server handle all these steps, so once I set this process up, I do not have to remember.

Now your Rails+Angular2 app is in the world of the internet! If you see 404 error on the home page in this particular example, don’t panic. That’s because we didn’t create a home page; we didn’t create a controller nor routing for ‘/’. But you can visit ‘/widgets’ page and ‘ng2-app/’ page to enjoy your app working.

#Conclusion

We’ve looked at step by step process for building Rails Application and Angular2 app from scratch using own CLI tools. And we’ve learned the Angular2 development workflow using ‘ng serve’ and API proxy. And we have found how to integrate the Angualr2 app into Rails app by serving the assets via Rails routing, the controller, and the view. Finally, we deployed the app to Heroku for the worlds to enjoy.

The main advantage of the described process is the loose coupling of Rails and Angular2 apps, which ensure future-proof. If your team expands and you’d want to create separate repositories for Rails and Anguar2 app, you only need to move ng2-app directory out of root directory and couple lines of configurations. And if you’d like to add more Angular2 apps under the same umbrella, you can do that by typing ‘ng new another-app’.

Although I used Angular2 in this example, the same process can be applied to any JavaScript Single Page App using bundlers like webpack or jspm or build tools like Grunt.

You can download entire code at github and view the working app here.
