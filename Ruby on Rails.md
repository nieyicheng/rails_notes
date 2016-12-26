1.控制器可以直接渲染HTML


```ruby
def hello
  render html: "hello,world!"
end

root 'controller_name#action_name'
这里,控制器的 称是 application,动作的 称是 hello
```

```
mv <source> <target> 移动文件(重命名)
cp <source> <target> 复制文件
rm <file> 删除符文件
rmdir 删除空目录
rm -rf <directory> 删除非空目录
git checkout -f 强制撤销这次改动，例如删除文件之类的
git commit -a -m"info" 这个命令集合了git add . & git commit -m "info" 
git commit -am "info"
但是必须只能在没有更改文件名字或者没有新增或删除文件名时使用，若新增删除了文件名则必须使用老方法。
git branch -d(D) 前一个命令可以删除已经合并后的分支，后一个命令即使没有合并也会强制删除
bundle install --without production 这是禁止在本地安装生产环境使用的GEM
```

Heroku 的推送必须在commit之后，也就是推送上heroku的都是commit的文件。

```
heroku login 登陆heroku 需要输入账号与密码
heroku key:add 添加SSH秘钥
heroku create 在heroku上创建应用
git push heroku master 把master分支推送到heroku上
heroku rename [name] 更改推送上去分支的名字，会相应更改.git config里的地址
heroku run rails db:migrate 在heroku上进行数据迁移
```

 						

图中各步的说明

1. 浏览器向/users发送请求；
2. Rails的路由把/users交给Users控制器的index动作处理
3. index动作要求User模型读取所有用户(User.all)
4. User 模型从数据库中读取所有用户;
5. User模型把所有用户组成的列表返回给控制器
6. 控制器把所有用户赋值给@users 变量，然后传入index视图;
7. 视图使用嵌入式Ruby把页面渲染成HTML;
8. 控制器把HTML送回浏览器



每一个 页面都对应不同的动作 ![Screen Shot 2016-08-06 at 11.45.32 PM](../../../images/Screen Shot 2016-08-06 at 11.45.32 PM.png)

​			
​		

#### validate

```ruby
validates :content, length: { maximum: 140} 限制长度 写在对应的model里
           presence: true 存在性验证
end

```



模型建立关联

```ruby
class User < ApplicationRecord 
has_many :microposts
end

class Micropost < ApplicationRecord
belongs_to :user
validates :content, length: { maximum: 140 }
end
```

我们可以把这种关联用图 表示出来，因为microposts 表中有user_id这一列，所以rails （通过Active

Record) 能把微博和各个用户关联起来。也就是两个model之间关联belongs_to的一方得建立另一方的model_id

![Screen Shot 2016-08-21 at 3.29.57 PM](../../../Desktop/Screen Shot 2016-08-21 at 3.29.57 PM.png)

```
rails g controller StaticPages home help
这个命令是制定生成home help两个动作并且会帮你配置 home help的路劲以及在 views里会帮你创建home help的html文件
这里创建controller的名字使用驼峰式大写 传入的时候就会变成蛇底式static_pages_controller.rb 这只是一种约定。其实在命令行中也可以使用蛇低式
rails g controller static_pages
```

#### routes

```ruby
  get 'static_pages/home'
  get 'static_pages/help'
  get 'static_pages/haha'
#get对应的是controller/action 如果没有action的话他会去找对应的views里的页面，但是必须要设置路由才能访问页面。#
#访问/static_page/home时候，rails会在staticpages控制器中寻找home动作，然后执行该动作，再渲染相应的试图，这里home动作是空的，所以访问/static_pages/home后只会渲染试图。home动作对应的试图是home.html.erb。
```

#### HTTP基本操作

GET、POST、PATCH、DELETE。这四个动词表示客户端电脑与服务器之间的操作，GET是最常用的HTTP操作，它的意思是“读取一个网页”。POST是第二种最常用的操作，当你提交表单时浏览器发送的就是POST请求。在Rails应用中，POST请求一般用于创建某个东西（不过HTTP也允许POST执行更新操作）。PATCH和DELETE，分别用于更新和销毁服务器中的某个东西。这两个操作没GET和POST那么常用，因为浏览器没有内建对这两种请求的支持。

#### TEST

在rails中模板就是视图

```ruby
group :test do
  gem 'rails-controller-testing'
  gem 'minitest-reporters'
  gem 'guard'
  gem 'guard-minitest'
end
#test的gem

#test/test_helper.rb
ENV['RAILS_ENV'] ||= 'test'
    require File.expand_path('../../config/environment', __FILE__)
    require 'rails/test_help'
    require "minitest/reporters"
    Minitest::Reporters.use!
class ActiveSupport::TestCase
# Setup all fixtures in test/fixtures/*.yml for all tests in alphabetical order. fixtures :all
      # Add more helper methods to be used by all tests here...
end
```





```ruby
require 'test_helper'
class StaticPagesControllerTest < ActionDispatch::IntegrationTest
test "should get home" do 
  get static_pages_home_url
  assert_response :success
end
  #get 表示测试期望着两个页面是普通的网页，可以通过get请求访问；:success 响应是对HTTP响应码的抽象表示。也就是说，下面这个测试的意思是：为了测试首页，向staticpages控制器中home动作对应的URL发起GET请求，确认得到是表示成功的响应码。
test "should get help" do 
  get static_pages_help_url 
  assert_response :success
end 
end
```



我们要使用 assert_select 方法分别为表 3.2 中的每个标题编写简单的测试,然 合并到代码清单 3.15 中。 assert_select 方法的作用是检查有没有指定的 HTML 标签。这种方法有时也叫“选择符”,从方法 可以看 出这一点

```ruby
assert_select "title", "Home | Ruby on Rails Tutorial Sample App"

```

这行代码的作用是检查有没有 <title> 标签,以及其中的内容是不是“Home | Ruby on Rails Tutorial Sample
App”字符串。把这样的代码分别放到三个页面的测试中,得到的结果如代码清单 3.24 所示。



```ruby
#使用方法setup去除重复代码
  def setup
    @base_title = "Ruby on Rails Tutorial Sample App"
  end

  test "should get home" do
    get static_pages_home_url
    assert_response :success
    assert_select "title", "Home | #{@base_title}"
  end
  
```



提供标题给页面使用

```ruby
<% provide(:title, "Help") %>
    <!DOCTYPE html>
    <html>
      <head>
<title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title> </head>
```

Rails别后的Ruby

```ruby
>> "foo" + "bar" # 字符串拼接
=> "foobar"
>> first_name = "Michael" # 变量赋值 => "Michael"
>> "#{first_name} Hartl" # 字符串插值 => "Michael Hartl"
>> puts "foo" # 打印字符串 foo
=> nil
>> print "foo" # 打印字符串,不换行
foo=> nil
>> print "foo\n" # 作用于 `puts "foo"` 一样 foo
=> nil
>> '#{foo} bar' # 单引号字符串不能进行插值操作
=> "\#{foo} bar"


    >> if s.nil?
    >>   "The variable is nil"
    >> elsif s.empty? #判断是否为空
    >>   "The string is empty"
    >> elsif s.include?("foo")
    >>   "The string includes 'foo'"#判断是否包含foo
    >> end
    => "The string includes 'foo'"


#布尔值还可以使用 &&(与)、||(或)和 !(非)运算符结合在一起使用:
    >> x = "foo"
    => "foo"
    >> y = ""
    => ""
    >> puts "Both strings are empty" if x.empty? && y.empty?
    => nil
    >> puts "One of the strings is empty" if x.empty? || y.empty?
    "One of the strings is empty"
=> nil
    >> puts "x is not empty" if !x.empty?
    "x is not empty"
    => nil

#在 Ruby 中一切都是对象,因此 nil 也是对象,所以它也可以响应方法。举个例子,to_s 方法基本上可以把 任何对象转换成字符串:
    >> nil.to_s
    => ""

#演示了 if 关键字的另一种用法:编写一个当且只当 if  面的表达式为真值时才执行的语句。还有个对应的
unless 关键字也可以这么用:
    >> string = "foobar"
    >> puts "The string '#{string}' is nonempty." unless string.empty?
    The string 'foobar' is nonempty.
    => nil

  
  #我们需要注意一下 nil 对象的特殊性,除了 false 本身之外,所有 Ruby 对象中它是唯一一个布尔值为“假” 的。我们可以使用 !!(读作“bang bang”)对对象做两次取反操作,把对象转换成布尔值:
    >> !!nil
    => false

```

```ruby
#在控制台中,可以像定义 home 动作(代码清单 3.8)和 full_title 辅助方法(代码清单 4.2)一样定义方 法。(在控制台中定义方法有点麻烦,我们通常在文件中定义,这里只是为了演示。)例如,我们要定义一 个 为 string_message 的方法,它有一个参数,返回值取决于参数是否为空:
    >> def string_message(str = '')
    >>
    >>
    >>
    >>
    >>
    >> end
    => :string_message
    >> puts string_message("foobar")
    The string is nonempty.
    >> puts string_message("")
    It's an empty string!
    >> puts string_message
    It's an empty string!
#如最后一个命令所示,我们可以完全不指定参数(此时可以省略括号)。因为def string_message(str = '') 中提供了参数的默认值,即空字符串。所以,str 参数是可选的,如果不指定,就使用默认值。
#注意,Ruby 方法不用显式指定返回值,方法的返回值是最 一个语句的计算结果。上面这个函数的返回值是 两个字符串中的一个,具体是哪一个取决于 str 参数是否为空。在 Ruby 方法中也可以显式指定返回值,下 面这个方法和前面的等价:


#还有一点很重要,方法并不关心参数的 字是什么。在前面定义的第一个方法中,可以把 str 换成任意有效 的变量 ,例如 the_function_argument,但是方法的作用不变:
    >> def string_message(the_function_argument = '')
>> >> >>
if the_function_argument.empty?
  "It's an empty string!"
else
      >>     "The string is nonempty."
    >>   end
    >> end
    => nil
    >> puts string_message("")
    It's an empty string!
    >> puts string_message("foobar")
    The string is nonempty.
```