Ruby on Rails 学习笔记
----
###Rails初始配置
####显示rails版本

	$ rails  -v

####显示所有可用Ruby版本

	$ rvm list

####显示当前Ruby版本使用的gemset

	$ rvm gemset list

####在gemsets之间切换

	$ rvm gemset use global
	$ rvm gemset use default

####建立一个项目专用的gemset并在其中安装rails

	$rvm use 'ruby-version'@'gemset_name' --create
	$gem install rails

####使用为项目建立的gemset

	$ rvm use 'ruby-version'@'gemset_name'
	$ rvm gemset list

####建立一个新的rails项目

	$ rails new 'project-name'

####使gemset跟随项目目录切换

	$ rvm use 'ruby-version'@'gemset_name' --ruby-version

--ruby-version参数会创建两个隐藏文件`.ruby-version`和`ruby-gemset`.每当进入项目目录,会自动切换到相应的rubyversion和gemset.


####启动rails测试服务器

	rails s[erver]

####Rails Gems

- **actionmailer** – framework for email delivery and testing
- **actionpack** – framework for routing and responding to web requests
- **activerecord** – framework for connections to databases
- **activeresource** – framework for manipulating data
- **activesupport** – utility classes and Ruby library extensions
- **bundler** – utility to manage gems
- **railties** – console commands and generators

#### Rails新项目默认包含的Gems

- **sqlite3** – adapter for the SQLite database
- **sass-rails** – enables use of the SCSS syntax for stylesheets
- **uglifier** – JavaScript compressor
- **coffee-rails** – enables use of the CoffeeScript syntax for JavaScript
- **jquery-rails** – adds the jQuery JavaScript library
- **turbolinks** – faster loading of webpages
- **jbuilder** – utility for encoding JSON data
####Learn-rails添加的Gems

Here are gems we’ll add to the Gemfile:
- **activerecord-tableless** – helps to use Rails without a database
- **figaro** – configuration framework
- **gibbon** – access to the MailChimp API
- **google_drive** – use Google Drive spreadsheets for data storage
- **high_voltage** – for static pages like “about”
- **simple_form** – forms made easy
We’ll add these gems for the Zurb Foundation front-end framework:
- **compass-rails** – support for Zurb Foundation
- **zurb-foundation** – front-end framework
We’ll also add utilities that make development easier:
- **better_errors** – helps when things go wrong
- **quiet_assets** – suppresses distracting messages in the log
- **rails_layout** – generates files for an application layout
####显示Gem所处位置
	$ gem which 'gem-name'
	# eg:
	$ gem which sass
	/Users/bearzk/.rvm/gems/ruby-2.0.0-p247/gems/sass-3.2.12/lib/sass.rb

#### Gemfile解释
	source ‘https://rubygems.org’ # 使用rubygems.org作为下载gems的源
	gem 'rails', '4.0.1' # 包含rails gem,而且一定是4.0.1版本
	gem 'sass-rails', '~> 4.0.0' # 使用4.0和4.1间的最新版本
	gem 'uglifier', '>= 1.3.0' # 使用高于1.3.0的版本

	# 在development环境下使用的gem
	group :development do
	  gem 'xxx'

编辑过Gemfile之后务必执行`bundle install`安装修改的部分.
可以使用`bundle install --without production`避免在本地安装生产环境所需的gems.

----
###配置Rails项目

除了`route.rb`之外,其它所有`config`修改之后都需要重启服务器`rails s`.

可以使用figaro gem来在`config/`成配置文件`application.yml`

	rails generate figaro:install

在生成的文件中可以添加修改包含本app中使用的变量和值以供其他config使用. e.g:

	# Add account credentials and API keys here.
	# See http://railsapps.github.io/rails-environment-variables.html
	# This file should be listed in .gitignore to keep your settings secret!
	# Each entry sets a local environment variable and overrides ENV variables 	in the Unix shell.
	GMAIL_USERNAME: Your_Username
	GMAIL_PASSWORD: Your_Password
	MAILCHIMP_API_KEY: Your_MailChimp_API_Key
	MAILCHIMP_LIST_ID: Your_List_ID
	DOMAIN_NAME: example.com
	OWNER_EMAIL: me@example.com

在其他`config`中如`config/environments/development.rb`中可以如下例使用上面的变量:

	config.assets.debug = true

	config.action_mailer.smtp_settings = { address: "smtp.gmail.com",
	  port: 587,
	  domain: ENV["DOMAIN_NAME"],
	  authentication: "plain",
	  enable_starttls_auto: true,
	  user_name: ENV["GMAIL_USERNAME"],
	  password: ENV["GMAIL_PASSWORD"]
	}
注意上面的内容要添加到最后的`end`之前.

----
###静态页面和路由
静态页面放在`public/`里.
路由可以在`config/route.rb`里修改.
	LearnRails::Application.routes.draw do
	  root to: redirect('/about.html')
	end

这里是访问网站的`root`也就是`/`时会自动跳转到`/about.html`.

	LearnRails::Application.routes.draw do
	  root to: 'visitors#new'
	end

这里访问`root`时就会自动启动`visitors`下的`new`方法.

----
###Rails Command Line

	$ rails c(onsole) [--sandbox]  #启动项目控制台[沙盒]
	$ rails s(erver)  #启动测试服务器
	$ rake  # rails make
	$ rails g(enerate)  #按照generator生成文件
	$ rails dbconsole  #启动数据库控制台
	$ rails new app_name  #创建新app

####scarffolding

scarffold会生成一系列应用所需的`model`,`controller`,`view`,`tests`以及`assets`.

	$ rails generate scaffold HighScore game:string score:integer
      invoke  active_record
      create    db/migrate/20120528060026_create_high_scores.rb
      create    app/models/high_score.rb
      invoke    test_unit
      create      test/models/high_score_test.rb
      create      test/fixtures/high_scores.yml
      invoke  resource_route
       route    resources :high_scores
      invoke  scaffold_controller
      create    app/controllers/high_scores_controller.rb
      invoke    erb
      create      app/views/high_scores
      create      app/views/high_scores/index.html.erb
      create      app/views/high_scores/edit.html.erb
      create      app/views/high_scores/show.html.erb
      create      app/views/high_scores/new.html.erb
      create      app/views/high_scores/_form.html.erb
      invoke    test_unit
      create      test/controllers/high_scores_controller_test.rb
      invoke    helper
      create      app/helpers/high_scores_helper.rb
      invoke      test_unit
      create        test/helpers/high_scores_helper_test.rb
      invoke  assets
      invoke    coffee
      create      app/assets/javascripts/high_scores.js.coffee
      invoke    scss
      create      app/assets/stylesheets/high_scores.css.scss
      invoke  scss
      create    app/assets/stylesheets/scaffolds.css.scss

`route`使用的是`resources`.

	resources :high_score

会生成7个包含了`CRUD`的`routes`:

	GET       /high_score
	GET       /high_score/new
	POST      /high_score
	GET       /high_score/:id
	GET       /high_score/:id/edit
	PATCH/PUT /high_score/:id
	DELETE    /high_score/:id

`resources`也可以使用`block`来nesting.

	resources :photos do
  		resources :comments
	end

会生成:

	GET       /photos/:photo_id/comments
	GET       /photos/:photo_id/comments/new
	POST      /photos/:photo_id/comments
	GET       /photos/:photo_id/comments/:id
	GET       /photos/:photo_id/comments/:id/edit
	PATCH/PUT /photos/:photo_id/comments/:id
	DELETE    /photos/:photo_id/comments/:id

`views`里生成的是一系列增删改查文件.

----
###动态页面

####MVC框架

**胖模型,瘦控制.**

- `class` **Visitor**` < ActiveRecord::Base` – 模型名大写单数.
- `class` **VisitorsController**` < ApplicationController`– 相应的控制器名用驼峰法复数+Controller构成.

**Visitor** -> **Visitors**

####MVC结构文件和目录命名方法:
- `app/models/visitor.rb` 模型文件名使用类名,但是小写.
- `app/controllers/visitors_controller.rb` 控制器文件名使用类名, 蛇形下划线命名.
- `app/views/visitors` 视图目录使用模型名复数小写.

####Model

####View
####Controller
可以使用`rails generate`或者`rails g`来产生assets, controller, model等。
	rails generate controller ControllerName action1 action2
这条命令会生成以下文件

	create app/controllers/controllername_controller.rb
	route   get "controllername/action1"
	route   get "controllername/action2"
	create app/views/controllername
	create app/views/controllername/action1.html.erb
	create app/views/controllername/action2.html.erb
	create test/controllers/controllername_controller_test.rb
	create app/helpers/controllername_helper.rb
	create test/helpers/controllername_helper_test.rb
	create app/assets/javascript/controllername.js.coffee
	create app/assets/stylesheets/modelname.css.scss

