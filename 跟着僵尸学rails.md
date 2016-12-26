#### 跟着僵尸学rails

```ruby
puts t[:id] #same as  puts t.id
puts t[:status] #same as puts t.status

#Create
t = Tweet.new(:status => "hahaha", :zombie => "jim")
t.save
#上面可以写成下面
Tweet.create(:status => "hahaha", :zombie => "jim")
```



```ruby
#Read
Tweet.find(2)  # Returns a single item
Tweet.find(3,4,5) # Returns an array
Tweet.first # Returns the first tweet
Tweet.last # Returns the last tweet
Tweet.all # Returns all the tweets
Tweet.count # Returns number of tweets
Tweet.order(:zombie) # ALL ordered by zombie 读取所有记录，并按名字的字母排序
Tweet.limit(10) # Only 10 tweets 限制只返回10条记录
Tweet.where(:zombie => "ash") #Only tweets by Ash 还可以只读取Ash发布的微博 
Tweet.where(:zombie => "ash").order(:zombie).limit(10) #method chaining 把不同方法放在一起执行“读取”操作。
```

```ruby
#Update
#做些改动，再保存记录
t = Tweet.find(2)
t.zombie = "EyeballChomper"
t.save

# 除此之外我们还可以传入一个hash,可以设定属性的值，再保存
t = Tweet.find(2)
t.attributes = {
  :status => "Can I munch your eyeballs?",
  :zombie => "EyeballChomper"
}
t.save

# 这个方法不仅可以设定属性值，还会保存记录
t = Tweet.find(2)
t.update_attributes(
  :status => "Can I munch your eyeballs?",
  :zombie => "EyeballChomper")

t.errors 会得到一个由错误信息组成的hash
t.errors[:status] 会返回特定属性产生的错误
```



```ruby
#Destroy
t = Tweet.find(2)
t.destroy

Tweet.find(2).destroy
#如果想销毁所有微博，只需要调用
Tweet.destroy_all
```



```ruby
#Model
#control去找table(数据表) 中间的纽带就是Model,controller里调用大写的Tweet时会先找到class Tweet(app/models/tweet.rb), Tweet 类继承自 ActiveRecord::Base 简单来说，这层关系会把类和数据表(table)联系起来，因此会在数据库中寻找复数形式,名为tweets的数据表
t = Tweet.find(3)
#Tweet是个类，用find(3)方法在数据库中查询id为3的记录，得到Tweet的一个实例，把获取数据存入对象，再赋值给变量t
```

```ruby
#验证提供给模型的数据
class Tweet < ActiveRecord::Base
  #确保保存之前指定了status的值
  validates :status, presence: true 
  
  #这行代码指定了属性，和对应验证规则
  validates :status, length: { minimum: 3 }
  
  #为了简便我们可以合成一行
  validate :status
  			presence: ture
  length: { minimum: 3 } #长度验证
  uniqueness: true #唯一性验证
  numericality: true #数字验证
  length: { minimum: 0, maximum: 2000} #范围验证
  format: { with: /.*/ } #格式验证
  acceptance: true
  confirmation: true #二次确认验证
end

```



```ruby
#Realation ship
#tweets和zombies建立关系,就可以在tweets表中添加 zombie_id 属性实现
app/models/tweet.rb
belongs_to :zombie

app/model/zombie.rb
has_many :tweets


```



```ruby
#View
#把头部和尾部放到application.html.erb中
<%= stylesheet_link_tag :all %> #这行代码会引入全部样式表，引入public/stylesheets目中中所有的样式表
生成如下显示的html 样式表
<link href="/stylesheets/scaffold.css" media="screen" rel="stylesheet" type="text/css" />

<%= javascript_inclued_tag :defaults %> #这行代码会引入默认提供的所有脚本 引入的文件都在public/javascripts目录中

<%= csrf_meta_tag %> #最后是夸张请求伪造元标签，这个标签可以防止黑客向网站提交垃圾内容。
```

```ruby
Tweet.all.each do |tweet|
#调用首字母大写的Tweet得到的是一个类，然后调用该类的 all 方法，all方法返回的是由所有微博组成的数组，遍历微博时，单篇微博存储在小写字母的变量 tweet 中。

#link
<%= link_to tweet.zombie.name, zombie_path(tweet.zombie) %>
<%= link_to tweet.zombie.name, tweet.zombie %>
#As with Tweets.shorter is better

  #如果想显示 no tweets found
  <% if tweets.size == 0 %>
<% if tweets.empty? %>
<em>No tweets found</em>
    <% end %>
调用empty? 方法可以简化这段代码

```



```ruby
#Controller
#在渲染视图前首先要经由控制器处理
#show method 对应的是 show view
#把对模型的访问都放在控制器层
#当收到请求时，首先经由控制器，执行show动作，然后渲染 show 视图
#实例变量在controller里使用@就可以在对应的view里用

#使用下面这个方法会让你在show页面显示 status
def show 
@tweet = Tweet.find(1)
render :action => "status"
end
```

```ruby
#在地址中包含请求参数或者发送POST请求 Rails 都会把参数存入params Hash中
怎么实现别人无法编辑其他用户的东西
def edit
@tweet = Tweet.find(params[:id])
if session[:zombie_id] != @tweet.zombie_id
  redirect_to tweets_path, notice: "Sorry, you can't edit this tweet"
end
end
#会话针对单个用户，用法类似hash，每次请求都会为当前用户存储会话，会话保存在cookie,我们要检查会话中的zombie_id是否和该微博的不一样

```

```ruby
#在控制器中怎么避免重复加入权限限制代码呢？
class TweetsController < ApplicationController
  before_action :get_tweet, only: [:edit, :update, :destroy]
  before_action :check_auth, only: [:edit, :update, :destroy]
  #这行代码告知控制器，执行各动作前调用该方法，我们可以指定只用于edit update destroy动作。
  
  def edit
  end
  
  def update
  end
  
  def destroy
  end
  
  private
    def get_tweet
    @tweet = Tweet.find(params[:id])
    end
  
  def check_auth
    if session[:zombie_id] != @tweet.zombie_id
      flash[:notice] = "Sorry,you can't edit this tweet"
      redirect_to tweets_path
    end
  end
  
  
end

```



```ruby
#Routes
resource :tweets #Creates what we like to call a "REST" ful resource,这行代码生成的就是rest式 资源,resour 方法会生成CRUD的所有地址
#controller name Tweets, Action name new
match "new_tweet" => "Tweets#new"
#new_tweet是地址,Tweets是控制器,new是动作，这样修改后，如果在浏览器访问new_tweet,会得到和/tweets/new页面一样的内容

#如果想在浏览器访问all和tweets一样的内容
match "all" => "Tweets#index", :as => "all_tweets"

#那么怎么在连接中使用 /all 地址呢？tweets_path不能用了，我们要在路由中指定 :as => "all tweets",这样就能使用all_tweets_path，这个地址会执行正确地页面。
<%= link_to "All Tweets", all_tweets_path %>


#如果你想要通过/all转向/tweets而不是直接渲染视图
#Redirect
#http://localhost:3000/all redirect to http://localhost:300/tweets

match "all" => redirect("/tweets")
match "google" => redirect("http://www.google.com/")
#也可以设置导向外站
```

```ruby
#如果我们想传入一个邮政编码查找特定区内的僵尸微博
/local_tweets/32828 
/local_tweets/32801
#find all zombie tweets in this zipcode


#/app/controllers/tweets_controller.rb
def index
if params[:zipcode] #我们可以在index动作中检查传入的邮码参数
@tweets = Tweet.where(:zipcode => params[:zipcode]) #如果传入了，查询该邮码地区的所有微博
else
@tweets = tweet.all #否则，就直接渲染页面显示所有微博
end

respond_to do |format|
format.html # index.html.erb
format.xml { render :xml => @tweets }
end
end

#实在这一功能要用到的路由是...
match "local_tweets/:zipcode" => "Tweets#index", :as => "local_tweets"
#这里:zipcode的意思是，不管这种地址传入什么，都把内容放入zipcode参数中

<%= link_to "Tweets in 32828", local_tweets_path(32828) %>
#同样地，如果指定了路由名称，例如 local_tweets，就可以在链接中使用 local_tweets_path.还要传入一个邮码。

```

```ruby
#我们希望用户名地址显示这个僵尸发布的所有微博，如果访问/greggpollack,会显示我的所有微博。
match ":name" => "Tweets#index", :as => "zombie_tweets"

<%= link_to "Gregg", zombie_tweets_path("greggpollack") %>

def index
if params[:name]
@zombie = Zombie.where(:name => params[:name]).first
@tweets = @zombie.tweets
else
@tweets = Tweet.all
end
end

```

