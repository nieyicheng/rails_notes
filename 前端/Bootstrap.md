container 他是居中的固定大小的布局 

container-fluid 自适应浏览器大小的布局 

thumbnail能显示div边框以及把里面的样式居中的作用

well是能把一个图片或者文字框住，有一个内嵌效果 well well-lg 大一号well well-sm 小一号

```ruby
 <%= link_to image_tag("rails.png", alt: "Rails logo”), 
'http://rubyonrails.org/' %>
```

会自动检索assets里images里的文件

#### 使用Carousel实现滚动图片广告

```html
<div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
  <!-- Indicators -->
  <ol class="carousel-indicators">
    <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
    <li data-target="#carousel-example-generic" data-slide-to="1"></li>
    <li data-target="#carousel-example-generic" data-slide-to="2"></li>
  </ol>

  <!-- Wrapper for slides -->
  <div class="carousel-inner" role="listbox">
    <div class="item active">
      <img src="..." alt="...">
      <div class="carousel-caption">
        ...
      </div>
    </div>
    <div class="item">
      <img src="..." alt="...">
      <div class="carousel-caption">
        ...
      </div>
    </div>
    ...
  </div>

  <!-- Controls -->
  <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
    <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
    <span class="sr-only">Previous</span>
  </a>
  <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
    <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
    <span class="sr-only">Next</span>
  </a>
</div>
```

 ![Screen Shot 2016-08-20 at 1.28.14 PM](../../../Desktop/Screen Shot 2016-08-20 at 1.28.14 PM.png)





#### Togglabel tabs

```html
<div>

  <!-- Nav tabs -->
  <ul class="nav nav-tabs" role="tablist">
    <li role="presentation" class="active"><a href="#home" aria-controls="home" role="tab" data-toggle="tab">Home</a></li>
    <li role="presentation"><a href="#profile" aria-controls="profile" role="tab" data-toggle="tab">Profile</a></li>
    <li role="presentation"><a href="#messages" aria-controls="messages" role="tab" data-toggle="tab">Messages</a></li>
    <li role="presentation"><a href="#settings" aria-controls="settings" role="tab" data-toggle="tab">Settings</a></li>
  </ul>

  <!-- Tab panes -->
  <div class="tab-content">
    <div role="tabpanel" class="tab-pane active" id="home">...</div>
    <div role="tabpanel" class="tab-pane" id="profile">...</div>
    <div role="tabpanel" class="tab-pane" id="messages">...</div>
    <div role="tabpanel" class="tab-pane" id="settings">...</div>
  </div>

</div>
```

#### 模态框

```html
<div class="modal fade" id="about">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
        <h4 class="modal-title">Modal title</h4>
      </div>
      <div class="modal-body">
        <p>One fine body&hellip;</p>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div><!-- /.modal-content -->
  </div><!-- /.modal-dialog -->
</div><!-- /.modal -->
```

模态框要放在body里。

```html
      <li><a href="#" data-toggle="modal" data-target="#about">关于</a></li>
      <button type="button" class="btn btn-primary btn-lg" data-toggle="modal" data-target="#about">
  Launch demo modal
</button>

```
模态框是默认隐藏的 想要启动模态框需要给模态框设置一个ID





进度条组件

进度条组件为当前工作流程或动作提供时时反馈。

```html
<div class="progress">
	<div class="progress-bar" style="width:60%";>60%</div>
  </div>

<!--最低值进度条-->
<div class="progress">
	<div class="progress-bar" style="min-width:20px; width:0%";>0%</div>
  </div>
<!--结合情景的进度条-->
<div class="progress">
	<div class="progress-bar progress-bar-success" style="min-width:20px; width:60%";>60%</div>
  </div>
<!--条纹状-->
<div class="progress">
	<div class="progress-bar progress-bar-success progress-bar-striped" style="min-width:20px; width:60%";>60%</div>
  </div>
<!--动画效果-->
<div class="progress">
	<div class="progress-bar progress-bar-success progress-bar-striped active" style="min-width:20px; width:60%";>60%</div>
  </div>
<!--堆叠效果-->
<div class="progress">
<div class="progress-bar progress-bar-success"
     style="min-width:20px; width:40%">40%</div>
<div class="progress-bar progress-bar-warning"
     style="min-width:20px; width:40%">40%</div>
<div class="progress-bar progress-bar-danger"
     style="min-width:20px; width:20%">20%</div>
</div>

```

媒体对象组件

媒体对象可以包含图片

```html
<div class="media">
  <div class="media-left media-middle(bottom,top)">
    <img src="img/haha.png" alt="" class="media-object">  
  </div>
  <div class="media-body">
    <h4 class="media-heading">标题
    </h4>
    <p>哈哈哈啊哈哈哈啊哈哈哈哈哈哈啊哈哈哈哈安徽安徽安徽安徽安徽安徽安徽安徽啊啊哈安徽安徽安徽安徽安徽啊哈啊哈哈啊哈哈安徽安徽安徽安徽好啊啊哈安徽安徽安徽安徽安徽啊啊哈安徽啊哈好
      
    </p>
  </div>
  
</div>
```

